https://bencane.com/2012/01/19/cheat-sheet-systemctl-vs-chkconfig/



## List processes {#list-processes}

**chkconfig**:

```
#
 chkconfig --list
```

**systemd**:

```
#
 systemctl list-units
```

## Enable a service {#enable-a-service}

**chkconfig**:

```
# chkconfig 
<
servicename
>
 on
```

**systemd**:

```
#
 systemctl 
enable
<
servicename
>
.service
```

## Disable a service {#disable-a-service}

**chkconfig**:

```
# chkconfig 
<
servicename
>
 off
```

**systemd**:

```
#
 systemctl 
disable
<
servicename
>
.service
```

## Start a service {#start-a-service}

**chkconfig**:

```
# service 
<
servicename
>
 start
```

**systemd**:

```
# systemctl start 
<
servicename
>
.service
```

## Stop a service {#stop-a-service}

**chkconfig**:

```
# service 
<
servicename
>
 stop
```

**systemd**:

```
# systemctl stop 
<
servicename
>
.service
```

### Check the status of a service {#check-the-status-of-a-service}

**chkconfig**:

```
# service 
<
servicename
>
 status
```

**systemd**:

```
# systemctl status 
<
servicename
>
.service
```



