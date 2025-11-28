
# syslog-ng

`syslogd` setup for remote logging over LAN.

I chose `syslog-ng` in particular as it is what
I am most familiar with at the moment, and did
not wish to spend a whole lot of time 
bootstrapping this.

## usage

The idea for this project is to be able to quickly bring up
a `syslogd` daemon that is ready for recording logs from
embedded hardware, such as a cheap router that is running
FreshTomato. In this scenario, there is often a logging
page that has a toggle for sending its log stream over to
a remote `syslogd` service.

This is very useful for one that is actively developing
runtime scripts for the firmware that are executed during
times in which `syslogd` may not yet have access to a
filesystem to record on until some condition is met.

- portal
    - `syslogd.fs1.home:514/udp`
    - `syslogd.fs1.home:601/tcp`
        - This route is disabled in [compose.yml](./compose.yml); re-enable by
        uncommenting the port under the `ports` declaration of the service.
    - `syslogd.fs1.home:6514/tcp`
        - This route is disabled in [compose.yml](./compose.yml); re-enable by
        uncommenting the port under the `ports` declaration of the service.

### deps

*...*

### docker setup

First, we must prepare our local environment to suit this
project by maintaining a local environment file called `.env`. This file must
never be checked into revision control! If you accidently push the file onto
a public medium, **all bits** considered sensitive must be discarded in full
and regenerated from scratch (by following this guide again).

So, without further ado, let us begin by creating our local environment file!

```shell
cp -av .env.dist .env
```

Next, use your favorite text editor and begin filling in the
variables inside of each file to your liking. However you choose
to manage your credentials for this sort of thing, be sure that the
passwords are kept somewhere safe and accessible in the case that they may
need to be revoked at some point in the future.

Finally, we can bootstrap the project! Cross your fingers, say
your prayers or however your typical ritual for this goes:

```shell
docker compose up -d
docker compose down
docker compose up -d --force-recreate
docker compose up -d service-server --force-recreate
```

```shell
# Check status of each service
docker compose logs #--since=5m -f

```shell
# Start and stop an individual service
docker start service-db # service name
docker stop service-proxy # service name
```

### inspecting the live container

```shell
docker exec -it syslog-ng-1 /bin/bash
```

### inspecting the remote log endpoint

By default, we do not output the logged messages that this container has
captured whatsoever. In order to see the messages collected, you must read
the stored log file on the container.

```shell
docker exec -it syslog-ng-1 tail -n50 -f /var/log/messages
```

## reference documents

[0]: https://github.com/i8degrees-dockerfiles/syslog-ng.git
[5]: https://hub.docker.com/r/linuxserver/syslog-ng

