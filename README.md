## MySQL Shell Backup

Used function:

`util.dumpInstance()`

Syntax:

`util.dumpInstance(outputUrl, [options])`

Description:

`dryRun: true` - display information about what would be dumped with the specified set of options, and about the results of MySQL Database Service compatibility checks (if the ocimds option is specified), but do not proceed with the dump.

`ocimds: true` - setting this option to true enables checks and modifications for compatibility with MySQL Database Service. The default is `false`.

`compatibility: ["strip_restricted_grants"]` - Remove specific privileges that are restricted by MySQL Database Service from `GRANT` statements, so users and their roles cannot be given these privileges (which would cause user creation to fail). 

`compression: "gzip"` - the compression type to use when writing data files for the dump. 

Trial run:
```
util.dumpInstance("/mnt/mysql_dump", {dryRun: true, ocimds: true, compatibility: ["strip_restricted_grants"]})
```
Run:
```
util.dumpInstance("/mnt/mysql_dump", {compression: "gzip", ocimds: true, compatibility: ["strip_restricted_grants"]})
```

