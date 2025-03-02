# 칼리 리눅스 실습 3

## ifconfig
 - ifconfig(window는 ipconfig) : 활성 네트워크 연결 조회
 - 네트워크 인터페이스 이름, 네트워크 카드 상태 플래그, mtu 정보, ip주소, 브로드캐스트 주소, 넷마스크, 루프백 주소(보통 127.0.0.1), wlan(무선 인터페이스) 등이 있다.
 - ifconfig 이더넷명 주소 netmask 주소 broadcast
 - ifconfig enp0 192.168.0.9 - enp0라는 이더넷에 아이피 192.168.0.9를 설정
 - ifconfig enp0 netmask 255.255.255.224 - enp0라는 이더넷에 서브넷 마스크만 255.255.255.224로 설정한다.
 - ifconfig enp0 broadcast 192.168.0.255 - enp0라는 이더넷에 브로드캐스트주소만 192.168.0.255로 설정한다.
 - ifconfig enp0 192.168.0.9 netmask 255.255.255.224 - enp0라는 이더넷에 아이피 192.168.0.9를 설정하고, 서브넷마스크를 255.255.255.224로 설정한다.
 - 이더넷(네트워크 인터페이스) 올리기/내리기
 - ifconfig 이더넷명 [up/down]
 - ifconfig enp0 up - enp0라는 이더넷을 올린다(활성화한다).
 - ifconfig enp0 down - enp0라는 이더넷을 내린다(비활성화한다)
 - ifconfig enp0 192.168.0.9 netmask 255.255.255.224 broadcast 192.168.0.255 up
 - enp0라는 이더넷에 아이피, 서브넷마스크, 브로드캐스트 주소를 다음 각 주소로 설정하고 올린다(활성화한다).

## 응용 : MAC 주소 변경
 - ifconfig eth0 down
 - ifconfig eth0 hw ether 00:11:22:33:44:55
 - ifconfig eth0 up

## DHCP 서버에서 새 IP 주소 할당
 - 리눅스에는 데몬(백그라운드 프로세스)으로 동작하는 동적 호스트 환경설정 프로토콜(DHCP) 서버를 구성할 수 있다.
 - 이는 dhcpd 또는 dhcp데몬이라고 부른다.
 - 칼리의 경우 dhclient eth0을 입력하면 새 주소를 할당할 수 있다.

## iwconfig
 - iwconfig : 무선 네트워크 장치를 점검할 수 있다.
 - iwconfig로 무선 확장을 갖는 네트워크 인터페이스,무선 확장 모드(Mode:Managed,monitor,promiscuous 등이 있음)가 뭔지도 알려줌.

## DNS(도메인 네임 시스템) 조작
 - dig : 대상 도메인에 대한 DNS 정보를 얻을 수 있는 방법을 제공한다.
 - 이 정보는 대상 네임서버의 IP주소, 대상의 이메일 서버, 모든 서브도메인 및 IP주소를 포함하고 있다.
 - 예를 들어 dig www.naver.com을 ns 매개변수와 함께 입력하면 naver.com의 네임서버는 ANSWER SECTION에 표시된다.
 - 매개변수 자리에 ns 말고도 mx(메일 교환 서버)도 있다.
 ```
dig hackers-arise.com ns 결과
; <<>> DiG 9.20.0-Debian <<>> hackers-arise.com ns
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 65375
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;hackers-arise.com.             IN      NS

;; ANSWER SECTION:
hackers-arise.com.      21600   IN      NS      ns7.wixdns.net. -> 네임서버 알 수 있음.
hackers-arise.com.      21600   IN      NS      ns6.wixdns.net.

;; Query time: 52 msec
;; SERVER: 8.8.8.8#53(8.8.8.8) (UDP)
;; WHEN: Fri Feb 14 03:20:10 EST 2025
;; MSG SIZE  rcvd: 92
 ```
 
 ```
 dig www.naver.com 결과
; <<>> DiG 9.20.0-Debian <<>> www.naver.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 19905
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;www.naver.com.                 IN      A

;; ANSWER SECTION:
www.naver.com.          10491   IN      CNAME   www.naver.com.nheos.com. -> CNAME으로 다시 들어가서 검색함.
www.naver.com.nheos.com. 129    IN      CNAME   www.naver.com.edgekey.net. -> 반복
www.naver.com.edgekey.net. 21012 IN     CNAME   e6030.a.akamaiedge.net. -> 반복
e6030.a.akamaiedge.net. 20      IN      A       23.40.44.189 -> IP주소 알아냄.

;; Query time: 44 msec
;; SERVER: 8.8.8.8#53(8.8.8.8) (UDP)
;; WHEN: Fri Feb 14 03:21:32 EST 2025
;; MSG SIZE  rcvd: 164
```

## DNS 서버 변경
 - /etc/resolv.conf로 명명된 평문 파일을 수정하면 된다.
 - 원래는 8.8.8.8 (google DNS서버)인데 수정을 하면 된다.
 - hosts라 불리는 시스템의 특수 파일도 도메인네임-ip주소 변환을 수행한다.
 - hosts는 /etc/hosts에 위치한다. DNS와 비슷하게 고유의 ip주소를 지정(도메인 네임 매핑)하는데 사용한다.
 - 해커의 입장에서 이는 dnsspoof같은 도구를 이용해 로컬 영역 네트워크에서 TCP 연결 사이의 트래픽을 악의적인 웹 서버로 전송하도록 유도할 수 있다.
 - 기본설정으로 127.0.0.1 localhost(루프백용), 127.0.1.1 kali등이 있다.
 