# Sometimes pg sucks

```sql
CREATE TABLE table_a (
    table_a_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE table_b (
    table_b_id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE table_c (
    table_a_id INT NOT NULL REFERENCES table_a(table_a_id),
    table_b_id INT NOT NULL PRIMARY KEY REFERENCES table_b(table_b_id)
);

CREATE TABLE table_d (
    table_a_id INT NOT NULL,
    table_b_id INT NOT NULL,
    FOREIGN KEY (table_a_id, table_b_id) REFERENCES table_c(table_a_id, table_b_id),
    PRIMARY KEY (table_a_id, table_b_id)
);

-- ERROR: there is no unique constraint matching given keys for referenced table "table_c"
```

## God, why?? Why the fuck do you need another fucking unique key index with constraint?? You have the fucking unique index on `table_c(table_b_id)`. Regardless of which fucking pair of `table_d(table_a_id, table_b_id)`, it will always point to one record in `table_c`, because `table_b_id` is globally unique in that table. JUST WHYYY???? PULL THE FUCKING RECORD USING THE FUCKING UNIQUE INDEX AND COMPARE THE REST OF THE FUCKING FOREIGN CONSTRAINT MANUALLY YOU DUMB BITCH!!!!
