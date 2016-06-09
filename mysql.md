# MySQL One Liners

### dump
```bash
mysqldump -u mysqluser -p mysqldatabase > db_backup.sql
```
### import
```bash
cat db_backup.sql | mysql -u mysqluser -p mysqldatabase
```
### dump with gzip
```bash
mysqldump -u mysqluser -p mysqldatabase | gzip > db_backup.sql.gz
```
### import with gzip
```bash
gunzip -c db_backup.sql.gz | mysql -u mysqluser -p mysqldatabase
or
gunzip < db_backup.sql.gz | mysql -u mysqluser -p mysqldatabase
```
### dump selected tables
```bash
mysql -u root -p database -e 'show tables like "tableName_%"' | grep -v Tables_in | xargs mysqldump database -u mysqluser -p > tables.sql
```

### dump only data
```bash
mysqldump --no-create-info --skip-triggers --extended-insert --lock-tables --quick DB TABLE > dump.sql
```

### Compare data between two databases and make a diff
```bash
mysqldump --skip-comments --skip-extended-insert --no-create-info -u mysqluser -pPassword targetDbName > targetDb.sql
mysqldump --skip-comments --skip-extended-insert --no-create-info -u mysqluser -pPassword sourceDbName > sourceDb.sql
diff targetDb.sql sourceDb.sql > diff.sql
```
