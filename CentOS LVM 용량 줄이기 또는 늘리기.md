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
/home에 대한 새로운 Logical Volume 생성 후 마운트 한다.
```shell
lvcreate -L 50GB -n home cs
```
이때 50GB는 이전 /home 용량에서 /에 추가로 할당할 용량을 빼고 난 후의 값이어야 한다.
```text
[root@localhost cs]# lvcreate -L 50GB -n home cs
WARNING: xfs signature detected on /dev/cs/home at offset 0. Wipe it? [y/n]: y
  Wiping xfs signature on /dev/cs/home.
  Logical volume "home" created.
```
파일 타입을 지정한다.
```shell
mkfs.xfs /dev/cs/home
```
```text
[root@localhost cs]# mkfs.xfs /dev/cs/home
meta-data=/dev/cs/home           isize=512    agcount=4, agsize=3276800 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=0 inobtcount=0
data     =                       bsize=4096   blocks=13107200, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=6400, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```
home을 마운트 한다.
```shell
mount /dev/mapper/cs-home
```
```text
[root@localhost cs]# df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             3.8G     0  3.8G   0% /dev
tmpfs                3.8G     0  3.8G   0% /dev/shm
tmpfs                3.8G  8.7M  3.8G   1% /run
tmpfs                3.8G     0  3.8G   0% /sys/fs/cgroup
/dev/mapper/cs-root   70G  3.2G   67G   5% /
/dev/sda2           1014M  213M  802M  21% /boot
/dev/sda1            599M  7.3M  592M   2% /boot/efi
tmpfs                770M     0  770M   0% /run/user/0
/dev/mapper/cs-home   50G  390M   50G   1% /home
```
## 4. /root 경로를 확장한다.
root 디렉토리에 남은 용량을 모두 100% 할당한다.
```shell
lvextend -r -l +100%FREE /dev/mapper/cs-root
```
```text
[root@localhost cs]# lvextend -r -l +100%FREE /dev/mapper/cs-root
  Size of logical volume cs/root changed from 70.00 GiB (17920 extents) to <964.52 GiB (246916 extents).
  Logical volume cs/root successfully resized.
meta-data=/dev/mapper/cs-root    isize=512    agcount=4, agsize=4587520 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=0 inobtcount=0
data     =                       bsize=4096   blocks=18350080, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=8960, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 18350080 to 252841984
```
```text
[root@localhost /]# df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             3.8G     0  3.8G   0% /dev
tmpfs                3.8G     0  3.8G   0% /dev/shm
tmpfs                3.8G  8.7M  3.8G   1% /run
tmpfs                3.8G     0  3.8G   0% /sys/fs/cgroup
/dev/mapper/cs-root  965G  9.5G  956G   1% /              --> 모두 할당되었다.
/dev/sda2           1014M  213M  802M  21% /boot
/dev/sda1            599M  7.3M  592M   2% /boot/efi
tmpfs                770M     0  770M   0% /run/user/0
/dev/mapper/cs-home   50G  390M   50G   1% /home
```