# CentOS 8 samba 서버 구축하기
(1) samba 설치하기
```shell
yum install samba
```
(2) samba가 사용하는 포트 개방하기
```shell
firewall-cmd --permanent --zone=public --add-service=samba
```
(3) 방화벽을 다시 로드하기
```shell
firewall-cmd --reload
```
(4) SMB와 NMB 서비스를 실행시킨다.
```shell
systemctl enable smb
```
```shell
systemctl start smb
```
```shell
systemctl enable nmb
```
```shell
systemctl start nmb
```
