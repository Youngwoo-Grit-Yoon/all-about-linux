# rsync를 이용한 백업 서버 구축
/root/data 디렉토리 내부의 모든 데이터를 /mnt/backup 디렉토리 내부로 복사
```shell
rsync -avz /root/data/ /mnt/backup/
```
Source 서버의 /root/data 디렉토리 내부의 모든 데이터를 Target 서버의 /mnt/backup 디렉토리 내부로 복사
```shell
rsync -avz root@server-1.test.co.kr:/root/data/ /mnt/backup/
```
Target에서 Source 서버 접속시 873 디폴트 포트가 아닌 22 SSHD 포트를 이용
```shell
rsync -avz -e ssh root@server-1.test.co.kr:/root/data/ /mnt/backup/
```
RSA 기반의 keyfile을 이용하여 비밀번호 입력 없이 원격 백업을 수행할 때  
이때 반드시 소스 서버에는 타겟 서버의 공개키가 등록되어 있어야 한다.
```shell
rsync -avzrt --delete -e "ssh -i /root/.ssh/id_rsa" root@10.1.15.135:/samba/PlatformDev2_Developer_Share_Folder/ /samba/PlatformDev2_Developer_Share_Folder/
```
```shell
rsync -avzrt --delete -e "ssh -i /root/.ssh/id_rsa" root@10.1.15.135:/samba/PlatformDev2_GenesysCloud_Share_Folder/ /samba/PlatformDev2_GenesysCloud_Share_Folder/
```