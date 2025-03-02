# 칼리 리눅스 실습 14

 - 시스템에서 다른 네트워크 장치를 스캔하고 연결하는 기능은 매우 중요하다.
 - 이는 와이파이 및 블루투스와 같은 무선 기술 표준을 이용하여 와이파이 및 블루투스 연결을 찾아 제어하는 것이 핵심이다.
 - 가장 일반적인 두 가지 기술인 와이파이, 블루투스를 알아본다.

# 와이파이 네트워크
 - AP(Access Point) : 무선 사용자가 인터넷에 엑세스하기 위해 연결하는 장치다.
 - 확장 서비스 세트 식별자(extended service set identifier,ESSID) : SSID와 같지만 무선 랜의 여러 AP에 사용할 수 있다.
 - 기본 서비스 세트 식별자(basic service set identifier,BSSID) : 각 AP의 고유한 식별자로 기기의 MAC 주소와 동일하다.
 - 서비스 세트 식별자(service set identifier,SSID) : 네트워크의 이름이다.
 - 채널(channel) : 와이파이는 14개 채널 중 하나에서 작동할 수 있다. 미국에서는 채널 1-11로 제한된다.
 - 신호강도(power) : 와이파이 AP에 가까울수록 신호강도가 더 강해지고, 연결은 크래킹하기 쉬워진다.
 - 보안(security) : 와이파이 AP에서 사용하는 보안 프로토콜은 WEP가 있었지만 결함이 있어서 WPA,WPA2-PSK(사전 공유키 사용)가 거의 모든 와이파이 AP에서 사용된다.
 - 모드(mode) : 와이파이는 관리, 마스터, 모니터 세 가지 모드 중 하나로 작동할 수 있다. 나중에 더 자세히 나온다.
 - 무선 범위(wireless range) : 미국에서 와이파이 AP는 0.5와트의 상한선에서 신호를 합법적으로 브로드캐스트 해야한다. (대략 100미터이다. 그러나 고성능 안테나에서는 20마일까지 확장 가능하다.)
 - 주파수(frequency) : 와이파이는 2.4GHz 및 5GHz에서 작동하도록 설계되었다. 최신 와이파이 AP와 무선 네트워크 카드는 종종 2개의 주파수를 모두 쓴다.

### SSID,BSSID,ESSID 차이
1. SSID (Service Set Identifier, 서비스 세트 식별자)
- 우리가 흔히 "Wi-Fi 이름"이라고 부르는 것
- 네트워크를 구별하기 위한 최대 32자의 문자(영문, 숫자, 특수문자 포함)
- 동일한 SSID를 가진 여러 개의 AP(Access Point)가 있을 수 있음

2. BSSID (Basic Service Set Identifier, 기본 서비스 세트 식별자)
- AP(Access Point, 무선 공유기)의 고유한 MAC 주소
- 하나의 SSID 아래 여러 개의 AP가 있을 때, 각 AP를 구별하는 역할
- 예를 들어, 같은 SSID를 가진 여러 AP가 있더라도 BSSID를 보면 각각 다른 기기임을 알 수 있음

3. ESSID (Extended Service Set Identifier, 확장 서비스 세트 식별자)
- 여러 개의 AP가 같은 SSID를 공유하면서 하나의 네트워크를 형성할 때 사용
- 예를 들어, 건물 전체에서 같은 SSID를 사용하면서 여러 개의 AP를 운영하는 경우, ESSID 개념이 적용됨

### Wi-Fi가 2.4GHz, 5GHz만 쓰는 이유?
Wi-Fi는 면허가 필요 없는 ISM(Industrial, Scientific, and Medical) 대역을 사용함.

1. 2.4GHz: 장애물 투과력이 좋고, 긴 거리까지 신호 전달 가능
2. 5GHz: 속도가 빠르고 간섭이 적지만, 도달 거리는 짧음
✅ ISM 대역이기 때문에 누구나 자유롭게 사용 가능
✅ 적절한 속도 & 도달 거리의 균형
✅ 기존 무선 기술(블루투스, 전자레인지 등)과의 조화를 고려

5GHz보다 더 높은 주파수(예: 6GHz)도 사용되지만, 도달 거리가 짧고 벽을 통과하기 어려움.
그래서 Wi-Fi는 2.4GHz와 5GHz를 주로 사용함!

📌 최근 Wi-Fi 6E는 6GHz 대역도 사용 가능해짐!
Wi-Fi 기술이 발전하면서 6GHz 대역을 활용하는 Wi-Fi 6E도 등장했음.

장점: 더 많은 채널 확보 가능, 간섭 최소화
단점: 도달 거리가 짧음

### 기본 무선 명령
 - ifconfig를 입력하면 기본 Ethernet 말고도 wlan0 Link encap:EthernetHWaddr 00:c0:ca:3f:ee:02 같은 것도 볼 수 있다.
 - 칼리에서 일반적으로 와이파이 인터페이스는 wlanX로 지정되며, X는 해당 인터페이스의 번호이다.
 - 무선 전용 명령어로 iwconfig도 있다. 입력하면 무선 인터페이스와 주요 데이터만 표시된다.
 ```
 lo        no wireless extensions. -> 내 컴퓨터의 경우 무선을 연결 안했다.(설정을 virtualbox에서 wired로 함.)
eth0      no wireless extensions.
만약 연결을 해준다면 밑과 같이 나온다.
wlan0 IEEE 802.11bg ESSID:off/any
        Mode:Managed Access Point:Not-Associated Tx-Power=20 dBm
        Retry short limit:7 RTS thr:off Fragment thr:off
        Encryption Key:off
        Power Management:off
 ```
 - 여기서 네트워크 카드라고 하는 무선 인터페이스와 사용된 무선 표준 ESSID가 꺼져있는지 여부, 모드를 포함하여 주요 데이터만 포함된다.

### WIFI 모드 설정정
 1. 마스터 모드 (Master Mode)
Access Point(AP) 역할을 하는 모드
이 모드에서는 Wi-Fi 어댑터가 무선 네트워크를 직접 생성함 (즉, 핫스팟처럼 동작)
다른 기기(클라이언트)들이 해당 네트워크에 연결할 수 있음
📌 특징
✅ AP(무선 공유기)처럼 작동
✅ 클라이언트들에게 IP 주소 할당 가능 (DHCP 지원 시)
✅ SSID를 생성하고, Wi-Fi 네트워크를 관리

📌 예시 : 스마트폰의 핫스팟 기능,Linux에서 hostapd를 이용한 소프트웨어 기반 AP 설정

2. 관리 모드 (Managed Mode)
일반적인 Wi-Fi 클라이언트 모드
우리가 보통 Wi-Fi를 사용할 때, 관리 모드로 동작함
AP(무선 공유기)에 연결해서 인터넷을 사용
📌 특징
✅ 클라이언트(사용자) 모드
✅ AP에 연결해서 데이터를 주고받음
✅ 대부분의 기기가 기본적으로 사용하는 모드

📌 예시 : 노트북, 스마트폰, 태블릿의 기본 Wi-Fi 동작 방식

3. 모니터 모드 (Monitor Mode)
패킷을 가로채서 분석할 수 있는 모드
네트워크에 직접 연결하지 않고, 공중에 떠다니는 Wi-Fi 신호를 감청할 수 있음
보안 테스트 및 네트워크 분석에 사용됨
📌 특징
✅ 특정 AP에 연결하지 않아도 모든 Wi-Fi 패킷 수집 가능
✅ 암호화되지 않은 패킷을 분석 가능
✅ 보안 전문가, 해커, 네트워크 엔지니어들이 많이 사용

📌 예시 : Wireshark, airmon-ng 등을 이용한 네트워크 패킷 분석,해킹 및 침입 테스트(예: 무선 네트워크 보안 점검)

- 연결하려는 와이파이 AP가 확실하지 않은 경우에 iwlist [인터페이스] [동작]으로 네트워크 카드가 연결할 수 있는 모든 무선 엑세스 지점을 볼 수 있다.
- iwlist wlan0 scan -> AP의 MAC 주소, 작동 중인 채널 및 주파수,품질,신호 레벨,암호화 키 활성 여부, ESSID 같은 주요 데이터와 함께 모든 주변 와이파이 AP가 포함된다.
- 해킹을 위해서는 타겟 AP의 MAC 주소(BSSID), 클라이언트의 MAC 주소(다른 무선 네트워크 카드), AP가 동작하는 채널이 필요하다.
- nmcli(network manager command line interface,네트워크 관리자 명령줄 인터페이스)도 와이파이 연결을 관리하는데 유용하다.
- 네트워크 인터페이스에 대한 고급 인터페이스를 제공하는 리눅스 데몬을 네트워크 관리자라고 한다.
- nmcli 명령을 사용하여 iwlist보다 더 많은 정보를 얻을 수 있다. nmcli dev wifi -> AP,SSID,모드,채널,전송속도,신호강도,보안 프로토콜을 포함하였다.
- 심지어 nmcli dev wifi connect [AP-SSID] password [비밀번호] 명령으로 와이파이도 연결할 수 있다.
- 무선 AP에 성공적으로 연결되면 iwconfig를 입력 했을 때 주파수 2.452GHz(예시)에서 작동,MAC,ESSID 등등을 알 수 있다.

## aircrack-ng를 활용한 와이파이 정찰
 - 와이파이 AP를 크래킹 하기 위해 대상 AP의 MAC주소(BSSID), 클라이언트의 MAC 주소, AP가 작동하는 채널이 필요하다.
 - aircrack -ng를 이용해보자. 일단 무선 네트워크 카드를 모니터 모드로 전환하여 카드가 통과하는 모든 트래픽을 볼 수 있도록 해야한다.
 - airmon-ng start|stop|restart [인터페이스] -> airmon-ng start wlan0 : wlan0을 모니터 모드로 전환한다.(stop은 중지,restart는 재시작)
 - 모니터 모드는 무선 네트워크 어댑터와 안테나 범위(300~500피트)의 무선 트래픽을 엑세스할 수 있다.
 - 모니터 모드로 바꾸면 wlan0mon 등으로 이름이 바뀐다.
 - 이제 airodump-ng로 브로드캐스팅 AP 및 해당 AP에 연결되거나 근방에 있는 클라이언트의 주요 데이터를 캡처하고 표시한다.
 - airodump-ng wlan0mon -> 무선 카드가 근처 AP 무선 트래픽에서 중요한 정보(BSSID,신호 강도,데이터 처리 속도,AP 이름,암호화 방식)를 출력한다.
 - 위쪽 화면은 BSSID,AP의 전원, 비컨 프레임 수, 데이터 처리율,채널(1~14),암호화 프로토콜,암호,인증 유형,브로드캐스트 AP에 대한 정보가 있다.
 - 와이파이 비밀번호를 크래킹하려면 3개의 터미널을 열어야 한다.

<chatgpt 질문문>
1. airodump-ng → 특정 AP의 패킷 캡처
airodump-ng -c 10 --bssid 01:01:AA:BB:CC:22 -w Hackers-ArisePSK wlan0mon
airodump-ng는 Wi-Fi 네트워크의 패킷을 캡처하는 도구야.
특정 AP(무선 공유기)에서 발생하는 패킷을 수집해서 .cap 파일로 저장하는 역할을 함.
목표: 해당 네트워크의 **핸드셰이크 패킷(WPA handshake)**을 잡아서 이후 크래킹에 사용하기 위함.
📌 옵션 설명:
-c 10 → 채널 10번에서만 패킷을 수집 (목표 네트워크의 채널)
--bssid 01:01:AA:BB:CC:22 → **해당 AP의 MAC 주소(BSSID)**를 지정하여 그 네트워크의 패킷만 수집
-w Hackers-ArisePSK → 패킷을 "Hackers-ArisePSK.cap" 파일로 저장
wlan0mon → 모니터 모드(Monitor Mode)로 동작 중인 인터페이스

2. aireplay-ng → 특정 클라이언트의 연결 끊기 (Deauthentication 공격)
aireplay-ng --deauth 100 -a 01:01:AA:BB:CC:22 -c A0:A3:E2:44:7C:E5 wlan0mon
aireplay-ng는 Wi-Fi 네트워크에 패킷을 인위적으로 전송하는 도구야.
여기서는 디인증(Deauthentication) 패킷을 100번 보내서 강제로 클라이언트의 연결을 끊는 공격을 수행함.
목표: 사용자가 다시 AP에 연결할 때 발생하는 핸드셰이크(WPA handshake) 패킷을 캡처하는 것.
📌 옵션 설명:
--deauth 100 → 100개의 디인증 패킷 전송 (클라이언트 강제 연결 해제)
-a 01:01:AA:BB:CC:22 → 공격 대상 AP의 MAC 주소(BSSID)
-c A0:A3:E2:44:7C:E5 → 공격 대상 클라이언트(사용자 기기)의 MAC 주소
wlan0mon → 모니터 모드 인터페이스
💡 이 공격의 효과: 특정 클라이언트를 강제로 Wi-Fi에서 끊음 → 사용자가 다시 연결하면서 WPA 핸드셰이크 패킷이 발생
이후 airodump-ng로 핸드셰이크 패킷을 잡아서 암호 크래킹에 사용

3. aircrack-ng → 핸드셰이크 패킷을 이용한 무차별 대입 공격(Brute Force)
aircrack-ng -w wordlist.dic -b 01:01:AA:BB:CC:22 Hackers-ArisePSK.cap
aircrack-ng는 캡처된 핸드셰이크 패킷을 분석해서 WPA/WPA2 암호를 크래킹하는 도구
**사전 공격(Dictionary Attack)**을 사용하여 여러 개의 비밀번호를 하나씩 대입하면서 맞는지 확인함.
📌 옵션 설명:
-w wordlist.dic → 비밀번호 목록(워드리스트) 파일 지정
-b 01:01:AA:BB:CC:22 → 해당 BSSID(AP)의 핸드셰이크 패킷을 대상으로 크래킹
Hackers-ArisePSK.cap → airodump-ng에서 캡처한 핸드셰이크 패킷 파일
💡 이 공격의 원리:
.cap 파일에 저장된 핸드셰이크 데이터를 분석
wordlist.dic의 비밀번호들을 하나씩 대입
맞는 비밀번호가 나오면 해당 Wi-Fi의 WPA/WPA2 암호가 노출됨

🔥 💀 전체 공격 흐름 정리
1️⃣ airodump-ng → 목표 네트워크의 패킷을 수집
2️⃣ aireplay-ng → 사용자의 연결을 끊고 다시 연결하게 유도 (핸드셰이크 캡처 유도)
3️⃣ aircrack-ng → 핸드셰이크 패킷을 분석하여 암호를 크래킹 (사전 대입 공격)

## 블루투스 감지 및 연결
 - 블루투스를 해킹한다는 것은 장치의 정보 손상,제어,정보 유출 등으로 이루어진다.
 - 블루투스 장치를 탐색하고 연결하는데 도움이 되는 몇 가지 기본 지식만을 알아본다.
 - 블루투스는 2.4~2.485GHz에서 작동하는 근거리 저전력 통신용 범용 프로토콜, 확산 스펙트럼, 초당 1600홉의 주파수 도약을 사용한다.
 - 블루투스 사양의 최소 범위는 10미터지만 특수 제작은 100미터 정도도 가능하다. 2 장치를 연결하는 것을 페어링(paring)이라고 한다.
 - 검색 기능 모드일때만 페어링이 가능한데, 검색 기능 모드일 때 블루투스 장치는 이름,클래스,서비스 목록,기술적인 정보를 전달한다.
 - 페어링 되면 서로 비밀 키,링크 키를 교환한다. 각각은 이 링크 키를 저장하므로, 향후 페어링에서 서로를 식별할 수 있다.
 - 모든 장치에는 고유한 48비트 식별자(MAC과 유사)와 제조업체에서 지정한 이름이 있다. 장치를 식별하고 엑세스할 때 유용한 데이터 조각이 된다.

### 블루투스 스캐닝 및 정찰
 - 리눅스에는 블루투스 신호를 스캔하는 데 사용할 BlueZ라는 블루투스 프로토콜 스택이 구현되어있다.
 - 만약 없다면 apt-get install bluez로 설치 가능하다.
 BlueZ 도구들:
 1. hciconfig : 블루투스 장치용 ifconfig다. 블루투스 인터페이스,장치의 사양을 쿼리하는데 사용한다.
 hci0: Type: BR/EDR Bus:USB
       BD Address : 10:AE:60:58:F1:37 ACL MTU: 310:10 SCO MTU: 64:8 
       .....
       TX bytes:42881 ....
 보다시피 블루투스 어댑터는 MAC 주소 10:AE:60:58:F1:37로 인식된다. 이 어댑터의 이름은 hci0이다.
 - 연결 활성화 확인을 위해 hciconfig hci0 up -> 명령이 성공적으로 실행되면 출력이 없고 새 프롬프트만 표시된다.
   이제 우리 컴퓨터의 어댑터가 활성화 되었으니 스캔을 해보자.

 2. hcitool : 장치가 순차적으로 작동할 수 있도록 장치 이름,장치 ID,장치 클래스 및 장치 클록 정보를 제공할 수 있다.
 hcitool scan : scanning... 72:6E:46:65:72:66 ANDROID BT 22:C5:96:08:5D:32 SCH-I535
 -> ANDROID, SCH 두 개의 장치를 찾았다. 주변에 어떤 장치가 있냐에 따라 다른 정보를 준다.

 hcitool inq : Inquiring... 24:C5:96:08:5D:32 clock offset:0x4e8b class:0x5a020c
 -> 장치의 MAC 주소, 클록 오프셋, 장치 클래스를 제공한다.
   클래스는 장치의 유형이며 https://www.bluetooth.org/en-us/specification/assigned-numbers/service-discovery/ 에서 확인이 가능하다.

 이 밖의 명령어는 hcitool --help로 알아볼 수 있다.
 3. hcidump : 블루투스 통신을 탈취할 수 있다.

### sdptool,l2ping
 - service discovery protocol(SDP)는 블루투스 서비스를 검색하기 위한 블루투스 프로토콜이며, BlueZ는 장치에서 제공하는 서비스를 검색하기 위한 sdptool을 제공한다.
 - sdptool browse [MAC 주소] : sdptool을 활용하여 hcitool inq에서 얻은 MAC주소를 검색할 수 있다.
 sdptool browse 24:C5:96:08:5D:32 -> 해당 도구로 스캐닝하여 저전력 속성 프로토콜인 ATT프로토콜을 지원하는것 등 정보를 얻을 수 있다.
 - l2ping : 근처 모든 장치의 MAC 주소를 수집하면, 검색 모드 여부에 관계 없이 ping을 보내 도달 가능한지 확인할 수 있다.
   이를통해 활성상태인지, 범위 내에 있는지 알 수 있다.
 - l2ping [MAC 주소] -c [보낼 패킷 수]
 l2ping 76:6E:46:63:72:66 -c 3 -> 3개의 패킷을 다음 MAC주소로 보내 도달 가능한지 볼 수 있다.
