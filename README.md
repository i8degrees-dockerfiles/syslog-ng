
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
    - `docker.fs1.home:514/udp`

### deps

*...*

### docker setup

First, we must prepare our local environment to suit this
project by maintaining a local environment file called `.env`. This file must
never be checked into revision control! If you accidently push the file onto
a public medium, **all bits** considered sensitive must be discarded in full
and regenerated from scratch (by following this guide again).

So, without further ado, let us begin by creating our local environment file!

```sh
cp -av .env.dist .env
```

Next, use your favorite text editor and begin filling in the
variables inside of each file to your liking. However you choose
to manage your credentials for this sort of thing, be sure that the
passwords are kept somewhere safe and accessible in the case that they may
need to be revoked at some point in the future.

```sh
PROJECT_SITE_URL=project.fs1.home
```

As I already have a private DNS setup on my LAN, I simply
visit my authoritative DNS panel and add an entry to the
**fs1.home** zone. Feel free to start off by using
**localhost** for this, orby adding an entry to your hosts
file at `/etc/hosts`.

```sh
# /etc/hosts
# Note that if you are using a reverse proxy that is hosted
# elsewhere, you may want to update the IP to reflect the
# address of the proxy host.
<hostIP> project.localhost
```

Finally, we can bootstrap the project! Cross your fingers, say
your prayers or however your typical ritual for this goes:

```sh
docker compose up -d
docker compose down
docker compose up -d --force-recreate
docker compose up -d service-server --force-recreate
```

```sh
# Check status of each service
docker compose logs #--since=5m -f

```sh
# Start and stop an individual service
docker start service-db # service name
docker stop service-proxy # service name
```

- bind mounts
  * `./mounts/...`

## reference documents

[0]: https://github.com/i8degrees-dockerfiles/syslog-ng.git
[5]: https://hub.docker.com/r/linuxserver/syslog-ng

