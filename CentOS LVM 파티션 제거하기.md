# CentOS LVM 파티션 제거하기
해당 문서에서는 cs-home 파티션을 제거한다고 가정한다.
## 1. home 언마운트
```shell
umount /dev/mapper/cs-home
```

## 2. Logical Volume(LVM) 제거
```shell
lvremove /dev/mapper/cs-home
```

## 3. fstab 파일 수정
```text
[root@glab-cloud01 ~]# vi /etc/fstab
#
# /etc/fstab
# Created by anaconda on Fri Feb 24 09:22:42 2023
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
/dev/mapper/cs-root     /                       xfs     defaults        0 0
UUID=1873e628-3dcd-430d-85ca-cd19c63cd0a6 /boot                   xfs     defaults        0 0
UUID=1D72-6148          /boot/efi               vfat    umask=0077,shortname=winnt 0 2
/dev/mapper/cs-swap     none                    swap    defaults        0 0
```
fstab 파일을 열람하여 cs-home 관련 라인을 제거하고 저장한다. 현재 상기 내용은 이미 cs-home 라인을 제거한 상태라서 보이지
않는다.

## 4. /etc/fstab 파일 수정한 것 반영
```shell
mount -a
```