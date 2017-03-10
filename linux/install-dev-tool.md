# Linux(CentOS) Development Tools Setup<br>(2017/03/10 기준 lastest version)

**Redis**
```
# cd /home/apps

# wget http://download.redis.io/releases/redis-3.2.8.tar.gz

# tar zxvf redis-3.2.8.tar.gz

# cd redis-3.2.8

# make

# mv /home/apps/redis-3.2.8 /home/apps/redis

# ln -s /home/apps/redis/src/redis-server /usr/local/bin/redis-server

# ln -s /home/apps/redis/src/redis-cli /usr/local/bin/redis-cli

# vim /home/apps/redis/redis.conf
---
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize yes
~
# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
pidfile /var/run/redis.pid
---

# cp /home/apps/redis/utils/redis_init_script /etc/init.d/redis

# vim /etc/init.d/redis
---
#!/bin/sh
#
# chkconfig: 2345 25 75
#
# Simple Redis init.d script conceived to work on Linux systems
# as it does use of the /proc filesystem.
~
PIDFILE=/var/run/redis.pid
CONF="/home/apps/redis/redis.conf"
---

# chkconfig redis on

# service redis start

# service redis stop
```

---

**Java**

[GET Download Url Ref](http://webdevnovice.tistory.com/7)
```
# cd /home/apps

# wget http://download.oracle.com/otn-pub/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jdk-8u121-linux-x64.tar.gz?AuthParam=1489157561_86dbe18f98e0f124e40423489019ba11

# tar zxvf jdk-8u121-linux-x64.tar.gz

# mv jdk-8u121-linux-x64.tar.gz\?AuthParam\=1489157561_86dbe18f98e0f124e40423489019ba11 jdk-8u121-linux-x64.tar.gz

# tar zxvf jdk-8u121-linux-x64.tar.gz

# mv jdk1.8.0_121 jdk

# ln -s /home/apps/jdk/bin/java /usr/local/bin/java
# ln -s /home/apps/jdk/bin/javac /usr/local/bin/javac
# ln -s /home/apps/jdk/bin/javap /usr/local/bin/javap

```
