# SQLite magics for IPython


First of all, you need a `sqlite.Connection` object.  There is a
SQLite magic to do that:
```sql
In [1]:
conn = %sqlite_create
```

To run SQL, use `%%sqlite_execute` cell magic:
```sql
In [2]:
%%sqlite_execute --commit conn
CREATE TABLE recipe (name, ingredients)
```

It is also possible to run multiple SQL statements:
```sql
In [3]:
%%sqlite_execute --script --commit conn

INSERT INTO recipe (name, ingredients)
VALUES ('broccoli stew', 'broccoli peppers cheese tomatoes');

INSERT INTO recipe (name, ingredients)
VALUES ('pumpkin stew', 'pumpkin onions garlic celery');

INSERT INTO recipe (name, ingredients)
VALUES ('broccoli pie', 'broccoli cheese onions flour');

INSERT INTO recipe (name, ingredients)
VALUES ('pumpkin pie', 'pumpkin sugar flour butter');
```

`%%sqlite_execute` shows resulting row in table format:
```sql
In [4]:
%%sqlite_execute conn
SELECT * FROM recipe
```
shows:
```
+---------------+----------------------------------+
|     name      |           ingredients            |
+===============+==================================+
| broccoli stew | broccoli peppers cheese tomatoes |
+---------------+----------------------------------+
| pumpkin stew  | pumpkin onions garlic celery     |
+---------------+----------------------------------+
| broccoli pie  | broccoli cheese onions flour     |
+---------------+----------------------------------+
| pumpkin pie   | pumpkin sugar flour butter       |
+---------------+----------------------------------+
```

It is also possible to show cursor object generated by Python code:
```python
In [5]:
c = conn.execute('SELECT * FROM recipe WHERE name LIKE ?', ['%pie%'])
%sqlite_show c
```
shows:
```
+--------------+------------------------------+
|     name     |         ingredients          |
+==============+==============================+
| broccoli pie | broccoli cheese onions flour |
+--------------+------------------------------+
| pumpkin pie  | pumpkin sugar flour butter   |
+--------------+------------------------------+
```

## Dependency

texttable

## License

ipython-sqlitemagic is licensed under the Simplified BSD License.
See the source code for more details.
