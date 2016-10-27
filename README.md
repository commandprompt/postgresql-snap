# postgresql-snap

This is a collection of snapcraft recipes for PostgreSQL 9.3, 9.4, 9.5 and 9.6 that can be used to create PostgreSQL snap packages.
The packages are maintained as a service to the community by Command Prompt, Inc. A PostgreSQL and Linux Professional services company.
You can find Command Prompt on the web at https://commandprompt.com/ .

## Get binaries

If you don't want to build the binaries but instead just want to install the
packages, run this command:

`$ sudo snap install postgresql$version

Where $version is one of 93,94,95 or 96.

## Build

Simply run `snapcraft` inside any of the postgresql9*/ directories.

## Install

If you want to install a local snap package run this command:

`$ sudo snap install --force-dangerous postgresql96_9.6.0_amd64.snap`

To install from Ubuntu Store simply run:

`$ sudo snap install postgresql96`

## postgres User

Traditionally, you would run PostgreSQL as an unprivileged postgres user. 
This user has to be created manually.

`$ adduser postgres`

Beware, that if you already have PostgreSQL installed on your system through an APT/PPA repository, the postgres system account most likely already exists. You must use a different system account in that case. For example, pgsql or postgsql. It can be an arbitrary name: admin, joe, malcolm, etc.

Do not use an existing postgres system account that was created during PostgreSQL installation from standard Ubuntu, PGDG or custom PPA repositories. If you want to run both snap package and traditional deb version of PostgreSQL, create a new account.

## Cluster Initialization

As postgres user run 

`$ postgresql96.initialize initdb` 

This will set up your environment, call initdb and create a default cluster.

To start PostgreSQL server run:

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile start`

Similarly, you can use pg_ctl to run usual commands: stop, restart, status, etc.

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile stop`

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile restart`

`$ postgresql96.pgctl -D /home/postgres/snap/postgresql96/x1/data -l /home/admin/snap/postgresql96/x1/logs/logfile status`

## Connect To PostgreSQL

`$ postgresql96.psql -h 127.0.0.1 -d postgres`

## Known Problems and Limitations

Snap format imposes a number of non-critical and more serious limitations:

* Only one locale is currently supported – en_US.UTF-8.
* There is no systemd service file for postgres daemon. PostgreSQL has to be managed manually by using pg_ctl.
* pg_ctl is run via a BASH wrapper to make it aware of a default system locale (en_US.UTF-8).
* psql is also run via a BASH wrapper to let it successfully write to HISTFILE (.psql_history)
* Kerberos, GSSAPI and Bonjour support is disabled.
* contrib modules are not included. These do not build due to snapcraft's make plugin inability to handle an included via a relative path Makefile inside another Makefile.

We intend to eliminate as many of these limitations as possible in future versions of these snapcraft recipes. Most of them are a result of various design decisions made by snapcraft developers that don't work well with how PostgreSQL build process works. However, snapcraft is a young, rapidly developing project and things may change sooner rather than later.
