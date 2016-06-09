# Liquibase - Source Control For Your Database

## Install
* [Download Liquibase](http://www.liquibase.org/download/index.html)
* Download appropriate java conncector. Eg. [MySQL connector](https://dev.mysql.com/downloads/connector/j/)

## liquibase.properties
Default value for parameters can be stored in a file called 'liquibase properties' that is read from the current working directory.
```bash
driver: com.mysql.jdbc.Driver
classpath: ./lib/mysql-connector-java-5.1.38-bin.jar
url: jdbc:mysql://localhost/dbName?useUnicode=true&characterEncoding=UTF-8
username: username
password: password
```
## Command Line
Liquibase can be run from the command line by running:
```bash
liquibase [options] [command] [command parameters]
```
### Reverse engineering schema
```bash
liquibase --changeLogFile="./changelog.xml" generateChangeLog
```
Note that this command currently has some limitations. It does not export the following types of objects:

* Stored procedures, functions, packages
* Triggers

### Standard Migrator Run
```bash
liquibase --changeLogFile="./changelog.xml" update
```
### Don’t execute changesets, save SQL to ./script.sql
```bash
liquibase --changeLogFile="./changelog.xml" updateSQL > ./script.sql
```
### "Tags" the current database state for future rollback.
```bash
liquibase tag 1.1
```
### Rolls back the database to the state it was in when the tag was applied.
```bash
liquibase rollback 1.0
```
## Diff between two databases
We need to combine liquibase with mysql command line tools

### Liquibase part
Example Parameters for liquibase.properties files
```bash
driver: com.mysql.jdbc.Driver
classpath: ./lib/mysql-connector-java-5.1.38-bin.jar
url: jdbc:mysql://localhost/targetDbName?useUnicode=true&characterEncoding=UTF-8
username: username
password: password
referenceUrl: jdbc:mysql://localhost/sourceDbName?useUnicode=true&characterEncoding=UTF-8
referenceUsername: username
referencePassword: password
```
Writes description of differences to standard out.
```bash
liquibase diff
```
Generate a changelog between source database to target database schema and stores it in changeLogFile
```bash
liquibase --changeLogFile="diffSchema.xml" diffChangeLog
```
Writes SQL to update database schema to current version to updateSchema.sql
```bash
liquibase --changeLogFile="diffSchema.xml" updateSQL > updateSchema.sql
```
Writes SQL to roll back the database schema to the current state after the changes in the changeslog have been applied
```bash
liquibase --changeLogFile="diffSchema.xml" futureRollbackSQL > rollbackSchema.sql
```
Export Data from Database **(This will export entire database data)**
```bash
liquibase --url="jdbc:mysql://localhost/sourceDbName?useUnicode=true&characterEncoding=UTF-8" \
--changeLogFile="entireData.xml" --diffTypes=data generateChangeLog
```
### MySQL part
Compare databases or tables
```bash
mysqldump --no-create-info --skip-triggers --extended-insert --lock-tables --quick DB TABLE > dump.sql
```
Compare data between two databases and make a diff
```bash
mysqldump --skip-comments --skip-extended-insert --no-create-info -u username -pPassword targetDbName > targetDb.sql
mysqldump --skip-comments --skip-extended-insert --no-create-info -u username -pPassword sourceDbName > sourceDb.sql
diff targetDb.sql sourceDb.sql > diff.sql
```
