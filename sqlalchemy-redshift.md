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

## Create a Temporary Table
In many environments (e.g., DBVisualizer, RPostgreSQL), I would just create a temporary table
using the hash method:
```sql
CREATE TABLE #temp as (SELECT someStuff FROM someTable);
```

However, when using SQLAlchemy, this can be an issue:
```python
ex("CREATE TABLE #temp as (SELECT someStuff FROM someTable);")

# This will throw an error saying that no such table exists...
qry("SELECT * from #temp LIMIT 10;")
```

The easiest work around is to just switch to using the "temporary table" method:
```python
ex("CREATE TEMPORARY TABLE temp1 as (SELECT someStuff FROM someTable);")

# This will throw an error saying that no such table exists...
qry("SELECT * from temp1 LIMIT 10;")
```

This works great! For some reason it annoys me though: I like my temporary tables
to be clearly marked with those dang hashtags.

Crazily enough, there is a little hack using a fstring that I figured out works. It's
interesting, but maybe not optimal.  For documentation's sake, here it is.

```python
ex("CREATE {hash}temp2 as (SELECT someStuff FROM someTable);".format(hash='#')

# This will throw an error saying that no such table exists...
qry("SELECT * from #temp2 LIMIT 10;")
```

The pro here is that whenever I use `qry` post creation, the temporary table is
clearly marked with '#' in the rest of the code.  However, the con is that
the CREATE statement itself is made slightly awkward in that the fstring's
brackets make it un-SQL-y looking.  Oh well!  
