# CentOS LVM /home 용량 줄이기 또는 / 용량을 늘리기
가상 머신에 CentOS를 처음 설치하면 기본 세팅으로 / 용량이 /home 용량에 비해 지나치게 부족하다. 따라서 /home 용량을 줄이고
/ 용량을 늘리는 방법을 설명한다.  
https://coconuts.tistory.com/832
## 1. home 경로 백업
해당 문서를 작성할 당시 /home 경로에는 아무것도 없으므로(사용자 계정을 생성한 적 없음) 생략함
## 2. home 언마운트
CentOS 8을 이제 막 설치하였을 당시 디렉토리 구조는 하기와 같다.
```text
[root@localhost ~]# df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             3.8G     0  3.8G   0% /dev
tmpfs                3.8G     0  3.8G   0% /dev/shm
tmpfs                3.8G  8.7M  3.8G   1% /run
tmpfs                3.8G     0  3.8G   0% /sys/fs/cgroup
/dev/mapper/cs-root   70G  3.2G   67G   5% /
/dev/sda2           1014M  213M  802M  21% /boot
/dev/mapper/cs-home  945G  6.7G  938G   1% /home
/dev/sda1            599M  7.3M  592M   2% /boot/efi
tmpfs                770M     0  770M   0% /run/user/0
```
하기 명령어를 실행하여 /home 디렉토리를 언마운트 한다.
```shell
umount /dev/mapper/cs-home
```
```text
[root@localhost /]# umount /dev/mapper/cs-home
[root@localhost /]# df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             3.8G     0  3.8G   0% /dev
tmpfs                3.8G     0  3.8G   0% /dev/shm
tmpfs                3.8G  8.7M  3.8G   1% /run
tmpfs                3.8G     0  3.8G   0% /sys/fs/cgroup
/dev/mapper/cs-root   70G  3.2G   67G   5% /
/dev/sda2           1014M  213M  802M  21% /boot
/dev/sda1            599M  7.3M  592M   2% /boot/efi
tmpfs                770M     0  770M   0% /run/user/0
```
## 3. home Logical Volume 삭제 후 재생성
일단 Logical Volume(cs-home)을 삭제한다.
```shell
lvremove /dev/mapper/cs-home
```
```text
[root@localhost /]# lvremove /dev/mapper/cs-home
Do you really want to remove active logical volume cs/home? [y/n]: y 
  Logical volume "home" successfully removed.
```
