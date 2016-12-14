# 参数解析

```shell
#!/bin/bash

usage() {
    echo "PostgreSQL simple backup script"
    echo "usage: `basename $0` [OPTIONS] [DBNAME]..."
    echo "options:"
    echo "  -b dir        store dump files there (default: \"$PGBK_BACKUP_DIR\")"
    echo "  -c config     alternate config file (default: \"$PGBK_CONFIG\")"
    echo "  -P days       purge backups older than this number of days (default: \"$PGBK_PURGE\")"
    echo "  -K number     minimum number of backups to keep when purging or 'all' to keep"
    echo "                everything (default: \"$PGBK_PURGE_MIN_KEEP\")"
    echo "  -D db1,...    list of databases to exclude"
    echo "  -t            include templates"
    echo 
    echo "  -h hostname   database server host or socket directory (default: \"${PGHOST:-local socket}\")"
    echo "  -p port       database server port (default: \"$PGPORT\")"
    echo "  -U username   database user name (default: \"$PGUSER\")"
    echo "  -d db         database used for connection (default: \"$PGBK_CONNDB\")"
    echo
    echo "  -q            quiet mode"
    echo
    echo "  -V            print version"
    echo "  -?            print usage"
    echo
    exit $1
}

now() {
    echo "$(date "+%F %T %Z")"
}

error() {
    echo "$(now)  ERROR: $*" 1>&2
}

die() {
    error $*
    exit 1
}

info() {
    [ "$quiet" != "yes" ] && echo "$(now)  INFO: $*" 1>&2
    return 0
}

warn() {
    echo "$(now)  WARNING: $*" 1>&2
}

args=`getopt "b:c:P:K:D:th:p:U:d:qV?" $*`
if [ $? -ne 0 ]
then
    usage 2
fi

set -- $args
for i in $*
do
    case "$i" in
	-b) CLI_BACKUP_DIR=$2; shift 2;;
	-c) PGBK_CONFIG=$2; shift 2;;
	-P) CLI_PURGE=$2; shift 2;;
        -K) CLI_PURGE_MIN_KEEP=$2; shift 2;;
	-D) CLI_EXCLUDE="`echo $2 | tr ',' ' '`"; shift 2;;
	-t) CLI_WITH_TEMPLATES="yes"; shift;;
	-h) CLI_HOSTNAME=$2; shift 2;;
	-p) CLI_PORT=$2; shift 2;;
	-d) CLI_CONNDB=$2; shift 2;;
	-U) CLI_USERNAME=$2; shift 2;;
	-q) quiet="yes"; shift;;
	-V) echo "pg_back version $version"; exit 0;;
	-\?) usage 1;;
	--) shift; break;;
    esac
done
```
