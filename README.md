# MySQL Shell (aka mysqlsh)

- [X] Intro
- [X] Cluster
- [ ] Cluster Crash Test
- [X] Create Test Database Table
- [X] Dump
- [X] Remove Test Database or Table
- [X] Recovery
- [X] Check Database Table
- [ ] Dump Authomatisation

+ [Cluster](https://github.com/y34r-z3r0/mysql-shell#cluster)
+ Cluster Crash Test
+ [Create Test Data](https://github.com/y34r-z3r0/mysql-shell#create-test-data)
+ [Dump](https://github.com/y34r-z3r0/mysql-shell#dump)
+ [Remove Test Data](https://github.com/y34r-z3r0/mysql-shell#remove-test-data)
+ [Recovery](https://github.com/y34r-z3r0/mysql-shell#recovery)
+ [Check Test Data](https://github.com/y34r-z3r0/mysql-shell#check-test-data)
+ Dump Authomatisation

# Intro

Here we looked at a really great MySQL infrastructure solution management using JavaScript for clustering instances and data backuping.

I express special gratitude to my colleague and mentor @github/vic-pay for help and support in the implementation of this project.

# Cluster

Functions

+ [dba.checkInstanceConfiguration](https://dev.mysql.com/doc/mysql-shell/8.0/en/check-instance-configuration.html)
+ [dba.createCluster](https://dev.mysql.com/doc/mysql-shell/8.0/en/create-cluster.html)
+ [addInstance](https://dev.mysql.com/doc/mysql-shell/8.0/en/add-instances-cluster.html)
+ [dba.getCluster](https://dev.mysql.com/doc/mysql-shell/8.0/en/monitoring-innodb-cluster.html)
+ [describe](https://dev.mysql.com/doc/mysql-shell/8.0/en/monitoring-innodb-cluster.html#describe-structure-innodb-cluster)
+ [status](https://dev.mysql.com/doc/mysql-shell/8.0/en/monitoring-innodb-cluster.html#check-innodb-cluster-status)

Syntax

+ `dba.checkInstanceConfiguration(instance)`
+ `dba.createCluster(name)`
+ `addInstance(instance)`
+ `dba.getCluster()`
+ `cluster.describe()`
+ `cluster.status()`

Connect to MySQL Shell

```
docker exec -it master-mysql mysqlsh
```

```
docker exec -it slave-mysql mysqlsh
```

<sub> Before creating a production deployment from server instances you need to check that MySQL on each instance is correctly configured. </sub>

Check whether the instance satisfies the requirements

```
dba.checkInstanceConfiguration('master-mysql:3306')
```

```
dba.checkInstanceConfiguration('slave-mysql:3306')
```


Create a password

```
var dbPass = "1234"
```

Create a cluster and add an instance

```
var cluster = dba.createCluster("fleetdm-cluster")
```

```
cluster.addInstance({user: "root", host: "slave-mysql", password: dbPass})
```

Check cluster

```
var cluster = dba.getCluster()
```

```
cluster.describe();
```

```
cluster.status();
```

# Create Test Data

Connect to MySQL

<sub> the password is in the variable $MYSQL_PASSWORD of docker-compose.yml. </sub>

```
mysql -p
```

Choose database

```
use y34rz3r0
```

Create test table

```
create table music_services (`id` int AUTO_INCREMENT PRIMARY KEY, `service` varchar(30) not null, price decimal not null);
```

Check table

```
show tables;
```

Create rows

```
insert music_services(service, price) values ('spotify', 9.99);
insert music_services(service, price) values ('apple_music', 10.99);
```

Сheck data

```
select * from music_services;
```

# Dump

<sub>the information_schema, mysql, ndbinfo, performance_schema, and sys schemas are always excluded from an instance dump.</sub>

Function
+ [util.dumpInstance](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-dump-instance-schema.html#mysql-shell-utilities-dump-opt-run)

Syntax

`util.dumpInstance(outputUrl, [options])`

Options

+ `dryRun: true` - display information about what would be dumped with the specified set of options, but do not proceed with the dump.

+ `ocimds: true` - display information about checks and modifications for compatibility with MySQL Database Service.

+ `compatibility: ["strip_restricted_grants"]` - remove specific privileges that are restricted by MySQL Database Service from `GRANT` statements, so users and their roles cannot be given these privileges (which would cause user creation to fail). 

<sub> you can choose whether or not to lock the instance for backup during the dump for data consistency by the "consistent" option. </sub>

+ `compression: "gzip"` - the compression type to use when writing data files for the dump. 

All options [here](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-dump-instance-schema.html#mysql-shell-utilities-dump-opt-control)

**Dry run**

<sub>the target directory must be empty before the export takes place.</sub>

```
util.dumpInstance("/mnt/mysql_dump", {dryRun: true, ocimds: true, compatibility: ["strip_restricted_grants"]})
```

**Run**

<sub>if the directory does not yet exist in its parent directory, the utility creates it.</sub>

```
util.dumpInstance("/mnt/mysql_dump", {compression: "gzip", ocimds: true, compatibility: ["strip_restricted_grants"]})
```

# Remove Test Data

<sub> keep in mind that we have dumped the whole instance, so you can delete either the table or the whole database </sub>

Remove table

```
drop table music_services;
```

Make sure the table is removed

```
show tables;
```

or delete whole database

```
drop database y34rz3r0;
```

Make sure the database is removed

```
show databases;
```

# Recovery

Function
+ [util.loadDump](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-load-dump.html#mysql-shell-utilities-load-dump-run)

Syntax

`util.loadDump(inputUrl, [options])`

**Dry run**

```
util.loadDump("/mnt/mysql_dump", {dryRun: true})
```

**Run**

```
util.loadDump("/mnt/mysql_dump")
```

# Check Test Data

Check database

```
show databases;
```

Check table

```
show tables;
```

Check data

```
select * from music_services;
```
