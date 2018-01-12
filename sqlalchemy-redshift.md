This page documents some helpful ways of interacting with a Redshift database
from the comfort of Python, using SQLAlchemy.

Note that I use "shortcut functions" that throughout this document, which are defined
like so:
```python
# qry
from pandas import read_sql_query as query

# ex
con = connect_to_redshift() # however you do it
ex = con.execute
```

## Grant public read permissions!
In R, using {RPostgreSQL}, one needn't worry about "begin" or "commit," however 
I've found this is not the case for SQLAlchemy.
```
ex("""
  BEGIN; 
  GRANT SELECT ON my_crazy_table TO PUBLIC 
  COMMIT;
""")
```

## Add a Column to Arbitrary Column Position
```
ex("""
  ALTER TABLE tbl RENAME TO old_tbl;
  ALTER TABLE old_tbl ADD COLUMN new_col <dataType>;
  UPDATE old_tbl SET new_col=<value> WHERE <condition>;
  CREATE TABLE tbl (col1 <dataType>, ..., new_col <dataType>, ..., colk <dataType>);
  INSERT INTO tbl (SELECT col1, ..., new_col, ..., colk FROM old_tbl);
  DROP TABLE old_tbl;
""")
```

