## percona-toolkit

[![Build Status](https://travis-ci.org/Oefenweb/ansible-percona-toolkit.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-percona-toolkit) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-percona--toolkit-blue.svg)](https://galaxy.ansible.com/list#/roles/6990)

Manage [percona-toolkit](https://www.percona.com/software/mysql-tools/percona-toolkit) in Debian-like systems.

#### Requirements

None

#### Variables

##### General

* `percona_toolkit_cron_jobs`: [optional, default: `[]`]: List of job declarations
* `percona_toolkit_cron_jobs.{n}.name`: [required]: Description of a crontab entry, should be unique, and changing the value will result in a new cron task being created (e.g. `duply-backup-etc`)
* `percona_toolkit_cron_jobs.{n}.job`: [required]: The command to execute (e.g. `/usr/bin/flock -n /var/lock/percona-toolkit-sheartbeat -c /usr/local/bin/percona-toolkit-heartbeat`)
* `percona_toolkit_cron_jobs.{n}.state`: [default: `present`]: Whether to ensure the job is present or absent
* `percona_toolkit_cron_jobs.{n}.day`: [default: `*`]: Day of the month the job should run (`1-31`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.hour`: [default: `*`]: Hour when the job should run (e.g. `0-23`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.minute`: [default: `*`]: Minute when the job should run (e.g. `0-59`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.month`: [default: `*`]: Month of the year the job should run (e.g `1-12`, `*`, `*/2`)
* `percona_toolkit_cron_jobs.{n}.weekday`: [default: `*`]: Day of the week that the job should run (e.g. `0-6` for Sunday-Saturday, `*`)

##### pt-heartbeat

* `percona_toolkit_heartbeat_defaults_file`: [optional]: Only read mysql options from the given file. You must give an absolute pathname.
* `percona_toolkit_heartbeat_host`: [optional]: Host to connect to (default localhost)
* `percona_toolkit_heartbeat_password`: [optional]: Password to use when connecting
* `percona_toolkit_heartbeat_port`: [optional]: Port number to use for connection
* `percona_toolkit_heartbeat_socket`: [optional]: Socket file to use for connection
* `percona_toolkit_heartbeat_user`: [optional]: User for login if not current user
* `percona_toolkit_heartbeat_daemonize`: [optional, default: `false`]: Fork to the background and detach from the shell. POSIX operating systems only.

##### pt-table-checksum

* `percona_toolkit_table_checksum_defaults_file`: [optional]: Only read mysql options from the given file. You must give an absolute pathname.
* `percona_toolkit_table_checksum_host`: [optional]: Host to connect to (default localhost)
* `percona_toolkit_table_checksum_password`: [optional]: Password to use when connecting
* `percona_toolkit_table_checksum_port`: [optional]: Port number to use for connection
* `percona_toolkit_table_checksum_socket`: [optional]: Socket file to use for connection
* `percona_toolkit_table_checksum_user`: [optional]: User for login if not current user

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
      - name: pt-heartbeat
        job: '/usr/bin/flock -n /var/lock/percona-toolkit-heartbeat -c /usr/local/bin/percona-toolkit-heartbeat'
        minute: 0
        hour: 0
      - name: pt-table-checksum
        job: '/usr/bin/flock -n /var/lock/percona-toolkit-table-checksum -c /usr/local/bin/percona-toolkit-table-checksum'
        minute: 0
        hour: 0
```

#### License

MIT

#### Author Information

Mark van Driel
Mischa ter Smitten

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-percona-toolkit/issues)!
