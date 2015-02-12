### Barrel Development Best Practices

# Database Best Practices and HowTo
 
## Importing a database from file
There are a number of ways to import a database from file. The easiest two ways are directly via command-line or from within the mysql command-line.

```bash
mysql -u username -p database_name < file.sql
```

```mysql
use database_name;
source path/to/file.sql;
```

## Indexing ([more](http://stackoverflow.com/questions/1108/how-does-database-indexing-work))

Given that creating an index requires additional disk space (277,778 blocks extra from the above example), and that too many indexes can cause issues arising from the file systems size limits, careful thought must be used to select the correct fields to index.

Indexes are used to speed up the necessity of searching through each and every single record. Consider adding indexes if they do not already exist. Most likely you would index columns that have relations or that need quick ordering when without indexing getting the exact record for a column and related columns would prove expensive in time and resources.
