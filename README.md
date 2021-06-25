## percona-toolkit

[![CI](https://github.com/Oefenweb/ansible-percona-toolkit/workflows/CI/badge.svg)](https://github.com/Oefenweb/ansible-percona-toolkit/actions?query=workflow%3ACI)
[![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-percona--toolkit-blue.svg)](https://galaxy.ansible.com/Oefenweb/percona_toolkit)

Manage [percona-toolkit](https://www.percona.com/software/mysql-tools/percona-toolkit) in Debian-like systems.

#### Requirements

* `software-properties-common` (will be installed)
* `dirmngr` (will be installed)

#### Variables

##### General

* `percona_toolkit_cron_jobs`: [optional, default: `[]`]: List of job declarations
* `percona_toolkit_cron_jobs.{n}.name`: [required]: Description of a crontab entry, should be unique, and changing the value will result in a new cron task being created (e.g. `pt-deadlock-logger`)
* `percona_toolkit_cron_jobs.{n}.job`: [required]: The command to execute (e.g. `/usr/bin/flock -n /var/lock/percona-toolkit-heartbeat -c /usr/local/bin/pt-heartbeat-wrapper`)
* `percona_toolkit_cron_jobs.{n}.state`: [default: `present`]: Whether to ensure the job is present or absent
* `percona_toolkit_cron_jobs.{n}.day`: [default: `*`]: Day of the month the job should run (`1-31`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.hour`: [default: `*`]: Hour when the job should run (e.g. `0-23`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.minute`: [default: `*`]: Minute when the job should run (e.g. `0-59`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.month`: [default: `*`]: Month of the year the job should run (e.g `1-12`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.weekday`: [default: `*`]: Day of the week that the job should run (e.g. `0-6` for Sunday-Saturday, `*`)

##### pt-deadlock-logger

* `percona_toolkit_deadlock_logger`: [optional, default: `{}`]: pt-deadlock-logger configuration declarations
* `percona_toolkit_deadlock_logger.defaults_file`: [optional]: Only read mysql options from the given file. You must give an absolute pathname.
* `percona_toolkit_deadlock_logger.host`: [optional]: Host to connect to (default localhost)
* `percona_toolkit_deadlock_logger.password`: [optional]: Password to use when connecting
* `percona_toolkit_deadlock_logger.port`: [optional]: Port number to use for connection
* `percona_toolkit_deadlock_logger.socket`: [optional]: Socket file to use for connection
* `percona_toolkit_deadlock_logger.user`: [optional]: User for login if not current user
* `percona_toolkit_deadlock_logger.daemonize`: [optional, default: `false`]: Fork to the background and detach from the shell. POSIX operating systems only.
* `percona_toolkit_deadlock_logger.opts`: [optional, default: `[]`]: Additional options
* `percona_toolkit_deadlock_logger.dest`: [optional]: DSN for where to store deadlocks configuration declarations
* `percona_toolkit_deadlock_logger.dest.host`: [optional]: Host of DSN for where to store deadlocks
* `percona_toolkit_deadlock_logger.dest.database`: [required]: Database of DSN for where to store deadlocks
* `percona_toolkit_deadlock_logger.dest.table`: [required]: Table of DSN for where to store deadlocks

##### pt-heartbeat

* `percona_toolkit_heartbeat`: [optional, default: `{}`]: pt-heartbeat configuration declarations
* `percona_toolkit_heartbeat.defaults_file`: [optional]: Only read mysql options from the given file. You must give an absolute pathname.
* `percona_toolkit_heartbeat.host`: [optional]: Host to connect to (default localhost)
* `percona_toolkit_heartbeat.password`: [optional]: Password to use when connecting
* `percona_toolkit_heartbeat.port`: [optional]: Port number to use for connection
* `percona_toolkit_heartbeat.socket`: [optional]: Socket file to use for connection
* `percona_toolkit_heartbeat.user`: [optional]: User for login if not current user
* `percona_toolkit_heartbeat.daemonize`: [optional, default: `false`]: Fork to the background and detach from the shell. POSIX operating systems only.
* `percona_toolkit_heartbeat.opts`: [optional, default: `[]`]: Additional options

##### pt-table-checksum

* `percona_toolkit_table_checksum`: [optional, default: `{}`]: pt-table-checksum configuration declarations
* `percona_toolkit_table_checksum.defaults_file`: [optional]: Only read mysql options from the given file. You must give an absolute pathname.
* `percona_toolkit_table_checksum.host`: [optional]: Host to connect to (default localhost)
* `percona_toolkit_table_checksum.password`: [optional]: Password to use when connecting
* `percona_toolkit_table_checksum.port`: [optional]: Port number to use for connection
* `percona_toolkit_table_checksum.socket`: [optional]: Socket file to use for connection
* `percona_toolkit_table_checksum.user`: [optional]: User for login if not current user
* `percona_toolkit_table_checksum.databases`: [optional]: Only checksum this list of databases (e.g. `['foo', 'bar']`)
* `percona_toolkit_table_checksum.ignore_databases`: [optional]: Ignore this list of databases (e.g. `['information_schema', 'common_schema']`)
* `percona_toolkit_table_checksum.opts`: [optional, default: `[]`]: Additional options

##### (Custom) scripts

* `percona_toolkit_script_map`: [default: `[]`]: Script declarations
* `percona_toolkit_script_map.{n}.src`: The local path of the file to copy, can be absolute or relative (e.g. `../../../files/percona-toolkit/usr/local/bin/pt-truncate-database.sh`)
* `percona_toolkit_script_map.{n}.dest`: The remote path of the file to copy (e.g. `/usr/local/bin/pt-truncate-database`)
* `percona_toolkit_script_map.{n}.owner`: The name of the user that should own the file (optional, default `root`)
* `percona_toolkit_script_map.{n}.group`: The name of the group that should own the file (optional, default `root`)
* `percona_toolkit_script_map.{n}.mode`: The mode of the file, such as 0644 (optional, default `0755`)

## Dependencies

None

## Recommended

* `percona-client` ([see](https://github.com/Oefenweb/ansible-percona-client))
* `percona-server` ([see](https://github.com/Oefenweb/ansible-percona-server))

#### Example(s)

##### Simple

```yaml
---
- hosts: all
  roles:
    - percona-toolkit
```

##### Create jobs

```yaml
---
- hosts: all
  roles:
    - percona-toolkit
  vars:
    percona_toolkit_cron_jobs:
      - name: pt-deadlock-logger
        job: '/usr/bin/flock -n /var/lock/percona-toolkit-deadlock-logger -c /usr/local/bin/pt-deadlock-logger-wrapper'
        minute: 0
        hour: 0
      - name: pt-heartbeat
        job: '/usr/bin/flock -n /var/lock/percona-toolkit-heartbeat -c /usr/local/bin/pt-heartbeat-wrapper'
        minute: 0
        hour: 0
      - name: pt-table-checksum
        job: '/usr/bin/flock -n /var/lock/percona-toolkit-table-checksum -c /usr/local/bin/pt-table-checksum-wrapper'
        minute: 0
        hour: 0
```

#### License

MIT

#### Author Information

* Mark van Driel
* Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-percona-toolkit/issues)!
