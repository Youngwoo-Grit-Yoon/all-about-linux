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
(5) 공유 폴더를 생성한다.
```shell
mkdir -p /samba/TechnicalDocs
```
```shell
chmod 777 /samba/TechnicalDocs
```
(6) SELinux를 설정한다.
```shell
setsebool -P samba_domain_controller on --> 해당 명령어는 한 번만 수행해주면 됨
```
```shell
setsebool -P samba_enable_home_dirs on --> 해당 명령어는 한 번만 수행해주면 됨
```
```shell
setsebool -P samba_export_all_rw on --> 해당 명령어는 한 번만 수행해주면 됨
```
```shell
chcon -t samba_share_t /samba/TechnicalDocs
```
(7) samba를 사용할 사용자 계정을 추가한다. 비밀번호를 입력한다.
```text
[root@glab-cloud01 samba]# smbpasswd -a youngwoo
New SMB password:
Retype new SMB password:
Added user youngwoo.
```
(8) /etc/samba/smb.conf 설정 파일에 하기와 같은 정보를 추가한다.
```text
[TechnicalDocs]
        comment = Technical Docs Directory of Hansol Inticube PlatformDev2
        path = /samba/TechnicalDocs
        public = yes
        writable = yes
        write list = youngwoo
        create mask = 0777
        directory mask = 0777
```
(9) 설정을 마쳤으면 하기 명령어를 순서대로 실행한다.
```text
chcon -t samba_share_t /samba/TechnicalDocs
systemctl restart smb
systemctl restart nmb
```