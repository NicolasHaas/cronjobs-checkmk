Role Name
=========

This role will deploy Checkmk monitored cronjobs.

To add a newly created Checkmk "Service" to the "Monitored services":
1. Open the Checkmk GUI.
2. Navigate to the host.
3. Click Host->Service-Configuration.
4. Under "Undecided services - currently not monitored" click the green "+" icon to move to monitored services.

Requirements
------------

Checkmk needs to be installed to "Bedag standards".

Role Variables
--------------
|cron_jobs[X].|Parameter|Default value|Description|
|-------|-------|-------|-------|
|name|string, **required**||Name of a cronjob.|
|user|string, **required**||User who should execute command.|
|cmd|string, **required**||Command that should be executed|
|enable_logging|boolean|false|If enabled will log cronjob output to `/var/lib/check_mk_agent/log/{{ item.user }}/{{ item.name }}/` and enable logrotate.|
|log.rotate|string|0|Log files are rotated count times before being removed or mailed to the address specified in a mail directive. If count is 0, old versions are removed rather than rotated.|
|log.size|string|10M|Log files are rotated only if they grow bigger then size bytes. If size is followed by k, the size is assumed to be in kilobytes. If the M is used, the size is in megabytes, and if G is used, the size is in gigabytes. So size 100, size 100k, size 100M and size 100Gare all valid.|
|log.maxage|string|30|Remove rotated logs older than <count> days.  The age is only checked if the logfile is to be rotated.  rotate -1 does not hinder removal.  The files are mailed to the configured address if maillast and mail are configured.|
|month|string|*|Month of the year the job should run (1-12, *, */2, and so on).|
|weekday|string|*|Day of the week that the job should run (0-6 for Sunday-Saturday, *, and so on).|
|day|string|*|Day of the month the job should run (1-31, *, */2, and so on).|
|hour|string|*|Hour when the job should run (0-23, *, */2, and so on).|
|minute|string|*|Minute when the job should run (0-59, *, */2, and so on).|

Variables should be passed to the role as a list as shown here:
```
cron_jobs:
  - name: 'example-cronjob'
    month: '*'
    weekday: '*'
    day: '*'
    hour: '*'
    minute: '*'
    cmd: 'logger "Test"'
    user: 'rapx'
    enable_logging: true
    log:
      rotate: 0
      size: 15M
      maxage: 14
```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```
- name: Playbook for checkmk monitored cronjobs
  hosts: dev
  become: true
  tasks:
    - name: Include the cronjobs-checkmk role
      include_role:
        name: cronjobs_checkmk
      vars:
        cron_jobs:
          - name: 'example-cronjob-root'
            minute: '*'
            cmd: 'logger "doing stuff";echo "stdout stuff"'
            user: 'root'
            enable_logging: true
          - name: 'example-cronjob-techuser123'
            hour: '12'
            cmd: 'dd if=/dev/urandom bs=1M count=10' # writing 10mb to stdout
            user: 'techuser123'
            enable_logging: true
            log:
              rotate: 0
              size: 15M
              maxage: 14
```
Additional Ressources
----------------------
https://docs.checkmk.com/latest/en/monitoring_jobs.html

https://docs.ansible.com/ansible/latest/collections/ansible/builtin/cron_module.html

https://github.com/logrotate/logrotate/blob/master/logrotate.8.in

License
-------

BSD

Author Information
------------------

nicolas.haas@bedag.ch
