# Central logging

## Receiver

1. Go to `/etc/rsyslog.conf` and enable the following lines

```
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")
```

2. Create a central log config at `/etc/rsyslog.d/central.conf`

3. Add the following to the config

```
$template RemoteLogs,"/var/log/central/%HOSTNAME%.log"
*.* ?RemoteLogs
& ~
```
4. Create logrotation (to avoid megafiles) by creating a file at `/etc/logrotate.d/central`, containing the following
```
/var/log/central/*.log
{
        rotate 4
        weekly
        missingok
        notifempty
        compress
        delaycompress
        sharedscripts
        postrotate
                invoke-rc.d rsyslog rotate >/dev/null 2>&1 || true
        endscript
}
```

5. Restart rsyslog using `sudo systemctl restart rsyslog`


## Setting up logging clients

1. Log into the clients
2. Add `*.* @@<loggin server ip or hostname>:514` e.g. `*.* @@192.168.0.100:514`  to the beginning of `/etc/rsyslog.conf`
3. Restart rsyslog


## (optional) Add liv logs package

1. Install lnav `sudo apt install lnav`
2. Open log watcher `lnav /var/log/central/*.log`
