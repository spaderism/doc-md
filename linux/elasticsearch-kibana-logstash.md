# Elasticsearch, Kibana, Logstash

---

## User Setting
```
--- create user
# useradd elasticsearch
# cat /etc/passwd | grep elasticsearch
# useradd kibana
# cat /etc/passwd | grep elasticsearch
# useradd logstash
# cat /etc/passwd | grep elasticsearch

--- change password
# echo 'elasticsearch' | passwd -- stdin elasticsearch
Changing password for user elasticsearch.
passwd: all authentication tokens updated successfully.
# echo 'kibana' | passwd -- stdin kibana
Changing password for user kibana.
passwd: all authentication tokens updated successfully.
# echo 'logstash' | passwd -- stdin logstash
Changing password for user logstash.
passwd: all authentication tokens updated successfully.

--- deny root login
# vi /etc/ssh/sshd_config
---
PermitRootLogin no
---

# service sshd restart

--- sudo setting
# vi /etc/sudoers
---
## Allow root to run any commands anywhere
root ALL=(ALL) ALL
elasticsearch ALL=(ALL) ALL
kibana ALL=(ALL) ALL
logstash ALL=(ALL) ALL

## Same thing without a password
%wheel ALL=(ALL) NOPASSWD:ALL (주석 제거)
---
```

---

## Install Guide

### Elasticsearch Install
```
# cd /home/apps
# curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.0.0.tar.gz
# tar -zxvf elasticsearch-5.0.0.tar.gz
# mv elasticsearch-5.0.0 elasticsearch
# chown -R elasticsearch:elasticsearch elasticsearch
# vim /home/apps/elasticsearch/bin/elasticsearch.sh
---
#!/bin/sh
#
# elasticsearch - this script start and stop the elasticsearch daemon
#

program="elasticsearch"
es_home="/home/apps/node1/bin"

green='\033[0;32m'
red='\033[0;31m'
nc='\033[0m' # No Color

start() {
    $es_home/elasticsearch -d -p es.pid
    printf "\nStarting $prog: [ ${green}OK${nc} ]\n\n"
}
stop() {
    kill `cat $es_home/es.pid`
    printf "\nStopping $prog: [ ${green}OK${nc} ]\n\n"
}
case "$1" in
    start)
        start && exit 0
        $1
        ;;
    stop)
        stop && exit 0
        $1
        ;;
    *)
        echo $"Usage: $0 {start|stop}"
        exit 2
esac
---

# chmod 755 elasticsearch.sh
```

---

### Kibana Install
```
# cd /home/apps
# curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.0.0-linux-x86_64.tar.gz
# tar -zxvf kibana-5.0.0-linux-x86_64.tar.gz
# mv kibana-5.0.0-linux-x86_64 kibana
# chown -R kibana:kibana kibana
```

---

### Logstash Install
```
# cd /home/apps
# curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.0.0.tar.gz
# tar -zxvf logstash-5.0.0.tar.gz
# mv logstash-5.0.0 logstash
# chown -R logstash:logstash logstash
```

## Setting Guide

### Elasticsearch
```
--- 메모리 설정
# vim /home/apps/elasticsearch/config/jvm.options
---
-Xms(OS메모리/2)g
-Xmx(OS메모리/2)g
---

--- swapoff
# sudo swapoff -a
# vim /etc/fstab -> swap 단어를 포함하는 라인 모두 comment out
# sudo sysctl vm.swappiness=1 (스왑 확률 줄이기)

# vim /etc/security/limit.conf
---
# allow user 'elasticsearch' mlockall
elasticsearch soft memlock unlimited
elasticsearch hard memlock unlimited

#allow user 'elasticsearch' nofile
elasticsearch soft nofile 100000
elasticsearch hard nofile 100000

#allow user 'elasticsearch' nproc
elasticsearch soft nproc 2048
elasticsearch hard nproc 2048
---
```
