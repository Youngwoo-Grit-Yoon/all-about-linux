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