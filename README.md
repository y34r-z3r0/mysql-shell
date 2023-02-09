### MySQL Shell Backup

Used function:

`util.dumpInstance()`

Syntax:

`util.dumpInstance(outputUrl, [options])`

Description:

`dryRun: true` - display information about what would be dumped with the specified set of options, and about the results of MySQL Database Service compatibility checks (if the ocimds option is specified), but do not proceed with the dump.

`ocimds: true` - setting this option to *true* enables checks and modifications for compatibility with MySQL Database Service. The default is *false*.

`compatibility: ["strip_restricted_grants"]` - 
