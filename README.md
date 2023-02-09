# MySQL Shell (aka mysqlsh)

- [ ] MySQL Shell Method Description 
- [ ] Cluster
- [ ] Cluster Diagnostic
- [ ] Create Database
- [X] Backup
- [ ] Remove Database
- [X] Recovery
- [ ] Check Database
- [ ] All Diagnostics (?)

# Backup

#### Command
`mysqlsh`

#### Function
[util.dumpInstance()](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-dump-instance-schema.html#mysql-shell-utilities-dump-opt-run)

<sub>The information_schema, mysql, ndbinfo, performance_schema, and sys schemas are always excluded from an instance dump.</sub>

#### Syntax

`util.dumpInstance(outputUrl, [options])`

#### Options

+ `dryRun: true` - display information about what would be dumped with the specified set of options, but do not proceed with the dump.

+ `ocimds: true` - display information about checks and modifications for compatibility with MySQL Database Service.

+ `compatibility: ["strip_restricted_grants"]` - remove specific privileges that are restricted by MySQL Database Service from `GRANT` statements, so users and their roles cannot be given these privileges (which would cause user creation to fail). 

+ `compression: "gzip"` - the compression type to use when writing data files for the dump. 

<sub> You can choose whether or not to lock the instance for backup during the dump for data consistency by the "consistent" option. </sun>

All options [here](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-dump-instance-schema.html#mysql-shell-utilities-dump-opt-control)

#### Dry run
```
util.dumpInstance("/mnt/mysql_dump", {dryRun: true, ocimds: true, compatibility: ["strip_restricted_grants"]})
```

<sub>The target directory must be empty before the export takes place.</sub>

#### Run
```
util.dumpInstance("/mnt/mysql_dump", {compression: "gzip", ocimds: true, compatibility: ["strip_restricted_grants"]})
```

<sub>If the directory does not yet exist in its parent directory, the utility creates it.</sub>

# Recovery

#### Function
[util.loadDump()](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-load-dump.html#mysql-shell-utilities-load-dump-run)

#### Syntax
`util.loadDump(inputUrl, [options])`

#### Dry run
```
util.loadDump("/mnt/mysql_dump", {dryRun: true})
```

#### Run
```
util.loadDump("/mnt/mysql_dump")
```

#### If you get error [53025](https://dev.mysql.com/doc/mysql-shell/8.0/en/mysql-shell-utilities-load-dump.html#mysql-shell-utilities-load-dump-errors)

add global variable in MySQL
```
set global local_infile=ON;
```
