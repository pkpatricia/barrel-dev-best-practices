### Barrel Development Best Practices

# Database Best Practices and HowTo

## Engines and Implementations
- When possible, always use either *XtraDB* and *InnoDB* when using the *MariaDB* or *other MySQL* engines, respectively. 

## Importing and Exporting Databases
There are a number of ways to import and export a database to file.

When entering the `mysql` console:
```mysql
use database_name;
source path/to/file.sql;
```
But the above can only be done locally while the following can be done remotely:
```bash
mysql -u username -p database_name < file.sql
```
You can also use the `mysqldump` utility to output and pipe to the `gzip` utility for a gzipped mysql dump file:
```bash
mysqldump <mysqldump options> | gzip > outputfile.sql.gz
```
When restoring from a compressed file, we can manually uncompress it first with `gunzip` utility:
```bash
gunzip -v outputfile.sql.gz
```
or again we can run in the same command line `mysql` and `gunzip`:

```bash
gunzip < outputfile.sql.gz | mysql <mysql options>
```
or using the `tar` utility:
```bash
tar -czvf -v outputfile.sql.tar.gz
```
and again we can run in the same command line mysqldump and gunzip:

```bash
gunzip < outputfile.sql.gz | mysql <mysql options>
```

## Indexing ([more](http://stackoverflow.com/questions/1108/how-does-database-indexing-work))

Given that creating an index requires additional disk space (277,778 blocks extra from the above example), and that too many indexes can cause issues arising from the file systems size limits, careful thought must be used to select the correct fields to index.

Indexes are used to speed up the necessity of searching through each and every single record. Consider adding indexes if they do not already exist. Most likely you would index columns that have relations or that need quick ordering when without indexing getting the exact record for a column and related columns would prove expensive in time and resources.
