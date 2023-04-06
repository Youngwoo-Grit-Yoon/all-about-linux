# tcpdump 사용법
인터페이스 eth0을 보여줌
```shell
tcpdump -i eth0
```
결과를 바이너리 파일로 저장
```shell
tcpdump -w tcpdump.log
```
저장한 파일을 읽음
```shell
tcpdump -r tcpdump.log
```
카운터 10개만 보여줌
```shell
tcpdump -i eth0 -c 10
```
TCP 80 포트로 통신하는 패킷을 보여줌
```shell
tcpdump -i eth0 tcp port 80
```
Source IP가 192.168.0.1인 패킷을 보여줌
```shell
tcpdump -i eth0 src 192.168.0.1
```
Destination IP가 192.168.0.1인 패킷을 보여줌
```shell
tcpdump -i eth0 dst 192.168.0.1

```
Source IP가 192.168.0.1이면서 TCP 80 포트인 패킷을 보여줌
```shell
tcpdump -i eth0 src 192.168.0.1 and tcp port 80
```