# Linux(CentOS) Setup
**Table of Contents**
- [Install procedure](#install-procedure)
- [Network setting](#network-setting)
- [Package install](#package-install)
- [gcc 4.8 upgrade](#gcc-48-upgrade)
- [Kernel Update](#kernel-update)
- [User Setting](#user-setting)

**iso file to usb boot disk convert [iso2usb](http://iso2usb.sourceforge.net/)**

---

## Install procedure
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

## Network setting
```
# vim /etc/sysconfig/network-scripts/ifcfg-eth0
---
ONBOOT=no -> yes
DEVICE=eth0
HWADDR=mac address
TYPE=Ethernet
UUID=uuid
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=dhcp
---

# service network restart
```


---

## Package install

```
개발자 도구 설치
bind-utils : 네임서버 관련 패키지
vim : 편집기 (vi 보다 향상된 편집기)
ntsysv : 부팅시 자동실행 서비스 관리
system-config-firewall-tui : 리눅스 방화벽 설정 패키지
system-config-network-tui : 리눅스 네트워크 설정 패키지

# yum -y update
# yum -y install wget
# yum -y groupinstall 'Development Tools'
# yum -y install bind-utils
# yum -y install vim
# yum -y install ntsysv
# yum -y install system-config-firewall-tui
# yum -y install system-config-network-tui

# reboot
```

---

## gcc 4.8 upgrade

```
-- gcc download
# wget http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-4.8.2/gcc-4.8.2.tar.bz2

-- gcc 압축해제
# tar -jxvf gcc-4.8.2.tar.bz2

-- gcc 컴파일에 필요한 라이브러리 다운로드. 일일이 받을필요 없음.
# cd gcc-4.8.2
# ./contrib/download_prerequisites

-- gcc 빌드
# ./configure --prefix=/usr/local/gcc-4.8.2 --enable-checking=release --enable-languages=c,c++ --disable-multilib
# make -j4

-- gcc 설치
# make install

-- gcc 버젼확인
# gcc --version

-- /etc/profile 수정
# vim /etc/profile
---
export GCC=/usr/local/gcc-4.8.2
export C_INCLUDE_PATH=$GCC/include/c++/4.8.2/
export CPLUS_INCLUDE_PATH=$GCC/include/c++/4.8.2/
export LD_LIBRARY_PATH=$GCC/lib64:$PPL/lib:$LD_LIBRARY_PATH
export PATH=$GCC/bin:$PATH
---
```

---

## Kernel Update
```
# rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
# rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
# yum --enablerepo=elrepo-kernel install kernel-lt

# /etc/grub.conf
---
default=0
timeout=5
splashimage=(hd0,0)/grub/splash.xpm.gz
hiddenmenu
title CentOS (3.10.105-1.el6.elrepo.x86_64)
        root (hd0,0)
        kernel /vmlinuz-3.10.105-1.el6.elrepo.x86_64 ro root=/dev/mapper/vg_centos-lv_root rd_NO_LUKS LANG=en_US.UTF-8 rd_LVM_LV=vg_centos/lv_swap rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=vg_centos/lv_root  KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM rhgb quiet
        initrd /initramfs-3.10.105-1.el6.elrepo.x86_64.img
---

# reboot
```

---

## User Setting
```
--- create user
# useradd testuser
# cat /etc/passwd | grep testuser

--- change password
# echo 'P@ssw0rd' | passwd -- stdin testuser
Changing password for user testuser.
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
user 추가..

## Same thing without a password
%wheel ALL=(ALL) NOPASSWD:ALL (주석 제거)
---
```
