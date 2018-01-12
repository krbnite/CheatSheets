This page documents some helpful ways of interacting with a Redshift database
from the comfort of Python, using SQLAlchemy.

## Grant public read permissions!
In R, using {RPostgreSQL}, one needn't worry about "begin" or "commit," however 
I've found this is not the case for SQLAlchemy.
```
con.execute("""
  BEGIN; 
  GRANT SELECT ON my_crazy_table TO PUBLIC 
  COMMIT;
""")
```

## Add a Column to Arbitrary Column Position
```
con.execute("""
  ALTER TABLE tbl RENAME TO old_tbl;
  ALTER TABLE old_tbl ADD COLUMN new_col <dataType>;
  UPDATE old_tbl SET new_col=<value> WHERE <condition>;
  CREATE TABLE tbl (col1 <dataType>, ..., new_col <dataType>, ..., colk <dataType>);
  INSERT INTO tbl (SELECT col1, ..., new_col, ..., colk FROM old_tbl);
  DROP TABLE old_tbl;
""")
```
