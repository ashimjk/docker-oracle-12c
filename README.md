[Oracle Standard Edition 12c Release 1 on Ubuntu](https://github.com/ashimjk/docker-oracle-12c)
===============================================

### Installation

    docker pull ashimjk/oracle-12c

Run with `8080` and `1521` ports opened:

    docker run -d -p 8080:8080 -p 1521:1521 ashimjk/oracle-12c

Run with data on host and reuse it:

    docker run -d -p 8080:8080 -p 1521:1521 -v /my/oracle/data:/u01/app/oracle ashimjk/oracle-12c

Run with Custom `DBCA_TOTAL_MEMORY` (in Mb):

    docker run -d -p 8080:8080 -p 1521:1521 -v /my/oracle/data:/u01/app/oracle -e DBCA_TOTAL_MEMORY=1024 ashimjk/oracle-12c

### Connect Database

Connect database with following setting:

    hostname: localhost
    port: 1521
    sid: xe
    service name: xe.oracle.docker
    username: system
    password: oracle

To connect using `sqlplus`:

    sqlplus system/oracle@//localhost:1521/xe.oracle.docker

Password for `SYS` & `SYSTEM`:

    oracle

### Connect to Oracle Application Express

Connect to web management console with following settings:

    http://localhost:8080/apex
    workspace: INTERNAL
    user: ADMIN
    password: 0Racle$

Connect to Oracle Enterprise Management console with following settings:

    http://localhost:8080/em
    user: sys
    password: oracle
    connect as sysdba: true

By Default web management console is enabled. To disable add env variable:

    docker run -d -e WEB_CONSOLE=false -p 1521:1521 -v /my/oracle/data:/u01/app/oracle ashimjk/oracle-12c
    # You can Enable/Disable it on any time

#### Upgrade Apex Up to v5.*

    docker run -it --rm --volumes-from ${DB_CONTAINER_NAME} --link ${DB_CONTAINER_NAME}:oracle-database -e PASS=YourSYSPASS sath89/apex install

Sample

    docker run -it --rm --volumes-from oracle --link oracle:oracle -e PASS=oracle quay.io/maksymbilenko/docker-oracle-apex:5.1.2 install

### Start with additional initial scripts

    docker run -d -p 1521:1521 -v /my/oracle/data:/u01/app/oracle -v /my/oracle/init/SCRIPTSorSQL:docker-entrypoint-initdb.d ashimjk/oracle-12c

By default, import from `docker-entrypoint-initdb.d` enabled only if you are initializing database (1st run).

If you need to run import at any case - add `-e IMPORT_FROM_VOLUME=true`

### Uses

Connect to `sqlplus`

    docker run -d -p 8080:8080 -p 1521:1521 --name oracle ashimjk/oracle-12c
    docker exec -it oracle /bin/bash
    # change user
    su oracle
    cd $ORACLE_HOME
    bin/sqlplus / as sysdba
    # now you are inside sqlplus
    select database_status from v$instance;

### Reference

- [Docker Oracle Apex](https://github.com/MaksymBilenko/docker-oracle-apex)

### Many Thanks To

- [Maksym Bilenko](https://github.com/MaksymBilenko/docker-oracle-12c)
- [Konnecteam](https://github.com/konnecteam/docker-oracle-12c)
