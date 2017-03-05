# Linux Setup

**iso file to usb boot disk convert [iso2usb](http://iso2usb.sourceforge.net/)**

---

![l1](/assets/l1.jpg)

---

![l2](/assets/l2.jpg)

---

![l3](/assets/l3.jpg)

---

![l4](/assets/l4.jpg)

---

![l5](/assets/l5.png)

---

![l6](/assets/l6.png)

---

![l7](/assets/l7.png)

---

![l8](/assets/l8.png)

---

![l9](/assets/l9.png)

---

![l10](/assets/l10.png)

---

![l11](/assets/l11.png)

---

**Network setting**
vi /etc/sysconfig/network-scripts/ifcfg-eth0
ONBOOT=no -> yes
```
DEVICE=eth0
HWADDR=mac address
TYPE=Ethernet
UUID=uuid
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=dhcp
```
service network restart

---

**Package install**
```
개발자 도구 설치
bind-utils : 네임서버 관련 패키지
vim : 편집기 (vi 보다 향상된 편집기)
ntsysv : 부팅시 자동실행 서비스 관리
system-config-firewall-tui : 리눅스 방화벽 설정 패키지
system-config-network-tui : 리눅스 네트워크 설정 패키지

[root@localhost ~] # yum -y groupinstall 'Development Tools'
[root@localhost ~] # yum -y install bind-utils
[root@localhost ~] # yum -y install vim
[root@localhost ~] # yum -y install ntsysv
[root@localhost ~] # yum -y install system-config-firewall-tui
[root@localhost ~] # yum -y install system-config-network-tui
[root@localhost ~] # reboot
```
