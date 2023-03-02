# 갑자기 SSH 로그인이 느려졌을 때 대처 방법
1. /etc/ssh/sshd_config 파일을 열람하여 GSSAPIAuthentication 옵션 값을 yes로 변경합니다.
```text
# GSSAPI options
GSSAPIAuthentication yes
GSSAPICleanupCredentials no
#GSSAPIStrictAcceptorCheck yes
#GSSAPIKeyExchange no
#GSSAPIEnablek5users no
```
2. 하기 명령어를 실행하여 sshd 서비스를 재시작 합니다.
```shell
systemctl restart sshd
```
3. 하기 명령어를 실행하여 systemd-logind 서비스를 재시작 합니다.
```shell
systemctl restart systemd-logind
```
