# postgresql-snap

This is a collection of snapcraft recipes for PostgreSQL 9.3, 9.4, 9.5 and 9.6 that can be used to create PostgreSQL snap packages.

## Build

Simply run `snapcraft` inside any of the postgresql9*/ directories.

## Install

If you want to install a local snap package run this command:

`$ sudo snap install --force-dangerous postgresql96_9.6.0_amd64.snap`

To install from Ubuntu Store simply run:

`$ sudo snap install postgresql96`

## postgres User

Normally, you would run PostgreSQL as an unprivileged postgres user. This user has to be created manually.

## Cluster Initialization

As postgres user run `$ postgresql96.initialize initdb` and look for an example of pg_ctl command in the end of the initdb output that shows how you can start PostgreSQL.

There are actually two ways to do it:

`$ /snap/postgresql96/x1/usr/bin/pg_ctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile start`

or

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile start`

The latter is a recommended way to run snap programs but both appear to work equally well. However, you should always first try postgresql9*.$PGCMD and execute any binaries directly only if the first method failed.

Similarly, you can use pg_ctl to run usual commands: stop, restart, status, etc.

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile stop`

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile restart`

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile status`

## Connect To PostgreSQL

`$ postgresql96.psql -h 127.0.0.1 -d postgres`

## Known Problems and Limitations

Snap format imposes a number of non-critical and more serious limitations:

* Only one locale is currently supported – en_US.UTF-8.
* psql cannot write to ~/.psql_history. The snap home: plug (or interface) that gives psql privileges to read and write inside home directories works with non-hidden files only.
* There is no systemd service file for postgres daemon. PostgreSQL has to be managed manually by using pg_ctl.
* pg_ctl is run via a BASH wrapper to make it aware of a default system locale (en_US.UTF-8).
* Kerberos, GSSAPI and Bonjour support is disabled.
* contrib modules are not included. These do not build due to snapcraft's make plugin inability to handle an included via a relative path Makefile inside another Makefile.

We intend to eliminate as many of these limitations as possible in future versions of these snapcraft recipes. Most of them are a result of various design decisions made by snapcraft developers that don't work well with how PostgreSQL build process works. However, snapcraft is a young, rapidly developing project and things may change sooner rather than later.
