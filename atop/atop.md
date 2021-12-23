# atop

## Basic usage
```bash
$ atop -afp 1
```

## Loggin settings
Verify logging frequency:
```bash
$ systemctl status atop
● atop.service - Atop advanced performance monitor
     Loaded: loaded (/lib/systemd/system/atop.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2021-12-23 09:49:13 GMT; 2h 43min ago
       Docs: man:atop(1)
   Main PID: 15329 (atop)
      Tasks: 1 (limit: 19058)
     Memory: 21.6M
     CGroup: /system.slice/atop.service
             └─15329 /usr/bin/atop -R -w /var/log/atop/atop_20211223 600
``` 
This is set to `600` seconds as can be seen above.

Verify config with going to the service file, from the info above:
```bash
$ cat /lib/systemd/system/atop.service
[Unit]
Description=Atop advanced performance monitor
Documentation=man:atop(1)

[Service]
Type=simple
ExecStart=/usr/share/atop/atop.daily
...

$ head /usr/share/atop/atop.daily
#!/bin/bash
# this is called - on sysvinit systems - at midnight by cron
# on systemd systems this is called from the systemd unit

LOGOPTS="-R"				# default options
LOGINTERVAL=600				# default interval in seconds
LOGGENERATIONS=28			# default number of days
```
