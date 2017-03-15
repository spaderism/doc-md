# Linux(CentOS) Development Tools Setup
**Table of Contents**
- [환경변수 등록](#%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EB%93%B1%EB%A1%9D)
- [Nginx](#nginx)
- [Redis](#redis)
- [Java](#java)
- [Tomcat](#tomcat)
- [Jenkins](#jenkins)
- [Node.js](#nodejs)
- [PM2](#pm2)
- [Git](#git)
- [MariaDB](#mariadb)

---

## 환경변수 등록
```
# vim /etc/profile(edit)
# source /etc/profile
```

---

## Nginx
```
# cd /home/apps
# wget http://zlib.net/zlib-1.2.11.tar.gz
# tar zxvf zlib-1.2.11.tar.gz
# cd zlib-1.2.11
# ./configure
# make
# make install

# cd /home/apps
# wget https://sourceforge.net/projects/pcre/files/pcre/8.38/pcre-8.38.tar.gz/download
# mv download pcre-8.38.tar.gz
# tar zxvf pcre-8.38.tar.gz
# cd pcre-8.38
# ./configure
# make
# make install

# cd /home/apps
# wget http://nginx.org/download/nginx-1.10.0.tar.gz
# tar zxvf nginx-1.10.0.tar.gz
# cd nginx-1.10.0
# ./configure --prefix=/home/apps/nginx
# make
# make install
```

---

## Redis
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

## Java

[GET Download Url Ref](http://webdevnovice.tistory.com/7)
```
# cd /home/apps
# wget http://download.oracle.com/otn-pub/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jdk-8u121-linux-x64.tar.gz?AuthParam=1489157561_86dbe18f98e0f124e40423489019ba11
# mv jdk-8u121-linux-x64.tar.gz\?AuthParam\=1489157561_86dbe18f98e0f124e40423489019ba11 jdk-8u121-linux-x64.tar.gz
# tar zxvf jdk-8u121-linux-x64.tar.gz
# mv jdk1.8.0_121 jdk
# ln -s /home/apps/jdk/bin/java /usr/local/bin/java
# ln -s /home/apps/jdk/bin/javac /usr/local/bin/javac
# ln -s /home/apps/jdk/bin/javap /usr/local/bin/javap

# vim /etc/profile
---
export JAVA_HOME=/home/apps/jdk
export PATH=$JAVA_HOME/bin:$PATH
---

# source /etc/profile

# echo $JAVA_HOME
```

---

## Tomcat
```
# cd /home/apps
# wget http://mirror.navercorp.com/apache/tomcat/tomcat-8/v8.5.11/bin/apache-tomcat-8.5.11.tar.gz
# tar zxvf apache-tomcat-8.5.11.tar.gz
# mv apache-tomcat-8.5.11 tomcat

# vim /etc/sysconfig/iptables (open port)
---
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
(reject 상위에 작성해야함)
---

# vim /etc/init.d/tomcat (write script)
---
#!/bin/sh
# chkconfig: 345 90 90
# description: init file for tomcat
# processname: tomcat

CATALINA_HOME=/home/apps/tomcat; export CATALINA_HOME
JAVA_HOME=/home/apps/jdk; export JAVA_HOME

[ -f $CATALINA_HOME/conf/server.xml ] && [ -f $CATALINA_HOME/conf/web.xml ] || exit 0

source /etc/profile

prog=Tomcat

start() {
    if [ -z "`ps -eaf | grep java|grep $CATALINA_HOME/bin`" ]; then
        echo -e "Starting $prog: "
        $CATALINA_HOME/bin/startup.sh
    else
        echo -e "Already Started $prog"
    fi
}

stop() {
    if [ -z "`ps -eaf | grep java|grep $CATALINA_HOME/bin`" ]; then
        echo -e "$prog was not started."
    else
        ps -eaf | grep java | grep $CATALINA_HOME/bin | awk '{print $2}' |
        while read PID
            do
                echo -e "Killing $PID ..."
                kill -9 $PID
                echo -e "$prog is being shutdowned."
            done
    fi
}

case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    restart)
        stop
        start
        ;;
    *)
        echo -e $"Usage: $0 {start|stop|restart}"
esac
---

# chmod +x /etc/init.d/tomcat
# chkconfig tomcat on
# service tomcat start
# service tomcat restart
# service tomcat stop
```

---

## Jenkins
```
# cp -R /home/apps/tomcat /home/apps/jenkins
# cd /home/apps/jenkins/webapps
# wget http://mirrors.jenkins-ci.org/war-stable/latest/jenkins.war
# unzip jenkins.war -d ROOT
# mkdir -p /home/apps/tmp
# mv jenkins.war /home/apps/tmp/jenkins.war
# cd /home/apps/jenkins/bin
# ./startup.sh

--- 초기 비밀번호 입력
# cat /root/.jenkins/secrets/initialAdminPassword

--- jenkins plugins 설치
Install suggested plugins 클릭

--- Create First Admin User
어드민 사용자 등록

--- 추가 plugins 설치
Jenkins 관리 -> 플러그인 관리
  - Checkstyle Plug-in
  - Cobertuna Plugin
  - Simple Theme Plugin

--- 테마 설정.
{{color-name}} : red, pink, purple, deep-purple, indigo, blue,
                 light-blue, cyan, teal, green, light-green, lime,
                 yellow, amber, orange, deep-orange, brown, grey, blue-grey
Jenkins 관리 -> 시스템 설정 -> Set the CSS field to the generated URL
: https://afonsof.com/jenkins-material-theme/dist/material-{{color-name}}.css
```

---

## Node.js
```
# cd /home/apps
# wget https://nodejs.org/dist/v6.10.0/node-v6.10.0-linux-x64.tar.gz --no-check-certificate
# mkdir -p node/versions
# mv node-v6.10.0-linux-x64.tar.gz node/versions
# cd /home/apps/node/versions
# tar zxvf node-v6.10.0-linux-x64.tar.gz
# cd /home/apps/node
# ln -s versions/node-v6.10.0-linux-x64/bin bin
# ln -s versions/node-v6.10.0-linux-x64/include include
# ln -s versions/node-v6.10.0-linux-x64/lib lib
# ln -s versions/node-v6.10.0-linux-x64/share share
# ln -s /home/apps/node/bin/node /usr/local/bin/node
# ln -s /home/apps/node/bin/npm /usr/local/bin/npm
```

---

## PM2
```
# npm install -g pm2@2.2.3
# ln -s /home/apps/node/bin/pm2 /usr/local/bin/pm2
# ln -s /home/apps/node/bin/pm2-dev /usr/local/bin/pm2-dev
```

---

## Git
```
# yum -y install git
# yum -y install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
# yum -y install  gcc perl-ExtUtils-MakeMaker
# yum -y remove git
# cd /usr/src
# wget https://www.kernel.org/pub/software/scm/git/git-2.12.0.tar.gz
# tar xzf git-2.12.0.tar.gz
# cd git-2.12.0
# make prefix=/usr/local/git all
# make prefix=/usr/local/git install
# echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/bashrc
# source /etc/bashrc
# git --version
git version 2.12.0
```

---

## MariaDB
```
# vi /etc/yum.repos.d/MariaDB.repo
---
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.1/centos6-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
---

# yum repolist (mariadb 확인)
필요시 MariaDB.repo 파일만 지우면 저장소가 제거된다.

# yum -y install MariaDB-server
→ DB를 실행하고 패스워드를 설정하라는 안내가 나온다.
→ 그런데 mysql 이라는 이름이 자꾸 보인다. (문제는 없다.)

# chkconfig mysql on
# chkconfig --list mysql

# service mysql start
# /usr/bin/mysqladmin -u root password 'P@ssw0rd'

접속확인
# mysql -u root -p
Enter Password:
MariaDB [(none)]> quit
```
