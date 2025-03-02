# 칼리 리눅스 실습 12

## 서비스의 시작, 중지, 재시작
 - 서비스란 사용자가 사용할 것을 기다리면서 백그라운드에서 실행되는 어플리케이션이다.
 - 리눅스에는 10개 정도의 서비스가 사전 설치되어 있다.(Apache,openSSH,MySQL/MariaDB,PostgreSQL 등등을 알아보자.)
 - service [서비스명] start|stop|restart로 서비스를 관리할 수 있다.
 - service apache2 start -> 아파치2 서비스 시작

### 아파치 웹 서버를 이용한 HTTP 웹 서버 생성
 - 아파치는 전세계 웹 서버의 55%이상의 점유율을 차지하고 있어서 잘 알고 있어야 한다.
 - 해커가 웹사이트,아파치,백엔드 데이터베이스의 세부사항을 이해해야한다. (XSS,malware,DNS악용 등)
 - sudo apt-get install apache2로 설치 -> service apache2 start를 하면 http://localhost/에 웹 페이지를 띄어준다.
 - 아파치의 /var/www/html/index.html을 이용해 웹사이트 html을 수정하기가 가능하다.
 ```
 <html>
 <body>
 <h1>studying linux</h1>
 <p> i want to go home.. </p>
 </body>
 </html>
 ```

### OpenSSH와 라즈베리 스파이 파이
 - SSH(secure shell)는 원격 시스템의 터미널로 안전하게 연결하게 해준다. (telnet의 대체제)
 - 암호화된 비밀번호로 사용자를 인증하고 모든 통신을 암호화한다. 가장 많이 쓰이는 리눅스 SSH가 OpenSSH다.
 - 라즈베리 파이(Raspberry Pi)는 영국의 라즈베리 파이 재단에서 개발한 초소형 컴퓨터 보드다.
   리눅스를 운영체제로 사용하며, 교육, IoT, 서버, 로봇 제어, 미디어 센터 등 다양한 용도로 활용할 수 있다.
 - 원격에서 ssh를 이용해 라즈베리 파이에 달린 카메라를 이용해 관찰해보는게 목표다.
 - 1. service ssh start -> SSH 사용
 - 2. https://www.raspberrypi.org/downloads/raspbian : 해당 라즈베리 파이는 Raspbian 운영체제를 구동하고 있다.
 이는 라즈베리 파이 CPU를 위해 특별히 포팅된 약간 다른 리눅스 배포판이다.
 - 3. $ pi > service ssh start (라즈베리 파이에서도 ssh 시작) -> ifconfig로 라즈베리 파이의 ip주소 얻는다.
 - 4. 칼리에서 ssh pi@192.168.1.101을 입력한다.
 - 5. raspberry(라즈베리 파이 기본 비밀번호)를 입력한다. (라즈베리 파이 접속을 위한 보안)
 - 6. sudo raspi-config -> 그래픽 메뉴가 시작된다. [6 Enable Camera]를 엔터 누르고 재부팅하면 감시를 할 수 있다.
 - raspistill -> 소형 라즈베리 스파이 파이로부터 사진 찍기 가능.
 - ex) raspistill -v -o firstpicture.jpg -> 사진을 찍고 JPG 파일로 저장한다. (-v : 풍부한 출력 요구,-o : 사진 이름 옵션)

### MySQL/MariaDB에서 정보 추출
 - MySQL은 데이터베이스 기반 웹 어플리케이션에서 가장 널리 사용되는 데이터베이스다.
 - 웹 2.0 기술의 시대에 대부분의 웹 사이트는 데이터베이스 기반이다. 즉, 대부분의 웹에 대한 데이터를 MySQL,MariaDB가 보유하고 있다.
 - MySQL,MariaDB는 오픈소스이며 일반 공중 라이선스(General Public License,GPL)을 따른다. 거의 모든 리눅스 배포판에서 적어도 하나는 사전에 설치되어 있다.
 - 1. https://www.mysql.com/downloads/에서 MySQL 다운로드 하기(설치 안 되어 있다면)
 - 2. service mysql start -> MySQL 서비스 시작
 - 3. mysql -u root -p -> password 입력(초기에는 없음)
 - SQL은 보통 select,union,insert,update,delete 라는 공통적인 명령이 존재한다.
   ex) select user, password from customers where user='admin'; -> customers 테이블에서 user값이 admin인 user,password 필드를 반환한다.
 - select user,host,password from mysql.user; -> 이미 존재하는 사용자와 그 사용자가 접속하는 서버(호스트),비밀번호를 알 수 있음.
 - 루트 사용자의 패스워드가 초기에 없으니 할당해야한다.
 - 1. show databases; -> 사용가능한 데이터베이스를 보여줌. information_schema,mysql,performance_schema,sys가 기본적으로 있음.
 - 2. use mysql; -> 이중에서 mysql로 접속한다.
 - 3-1. update user set password = PASSWORD("kali") where user='root'; -> 루트의 mysql 계정을 kali로 정함.(MySQL 5.6 이하)
 - 3-2. set PASSWORD for 'root'@'localhost' = PASSWORD("kali"); -> 루트의 mysql 계정을 kali로 정함.(MySQL 5.7 이상)

### 데이터베이스 접근해서 유용한 정보들 보기기
 - mysql -u [사용자명] -p : MySQL 데이터베이스에 접속하기 위한 명령어이다. 이 경우 -p 뒤에 ip가 없으면 로컬호스트 접속이다.
 ex) mysql -u root -p -> root 계정으로 MySQL에 접속,실행 후 비밀번호 입력을 요구함
 - mysql -u myuser -p -h 192.168.1.100 -> localhost가 아닌 원격 서버(192.168.1.100)접속, -h는 호스트를 지정하는 옵션이다.
 - 비밀번호를 입력해 뚫었다고 가정해보자. 그렇다면 이제 데이터베이스에 접속해 가치있는 정보들을 살펴볼 수 있다.
 - show databases; (데이터베이스들 확인) -> use creditcardnumbers; database changed -> creditcardnumbers라는 db는 가치있어 보인다. 물론 이름을 어렵게 해놓으면 더 찾긴 해야한다.
 - show tables; -> 크레딧카드 안의 테이블들을 봐보자. 그러면 cardnumbers 같은 테이블들을 찾을 수 있다.
 - 테이블이 너무 많다면 describe cardnumbers; -> 테이블의 구조를 볼 수 있다.
 - select 명령으로 조회해보자. SELECT columns FROM table; -> select문은 다음과 같은 구조를 지닌다.
   ex) SELECT * FROM cardnumbers; -> cardnumbers 안의 테이블을 모두 본다.

### PostgreSQL과 메타스플로이트
 - PostgreSQL 또는 Postgres는 규모를 쉽게 확장할 수 있고 고부하 처리가 가능하다는 점에서 인터넷 기반 애플리케이션에서 널리 사용되는 오픈소스 관계형 db다.
 - PostgreSQL는 칼리에 기본적으로 설치된다. 만약 안 되어있따면 sudo apt-get postgres install로 설치가 가능하다.
 - service postgresql start -> PostgreSQL 시작
 - msfconsole -> 메타스플로이트를 가동시킨다. 시작이 된다면 msf > 프롬프트를 볼 수 있다.
 - msfdb init -> PostgreSQL을 설정하고 시스템의 메타스플로이트 활동에서 정보를 저장한다.
 (Metasploit은 많은 데이터를 다루기 때문에, 공격 대상 정보(호스트, 포트, 취약점 등)를 저장하고 관리하려면 데이터베이스가 필요하다.)
 - 루트로 Postgres에 로그인한다. 여기서는 su 명령을 실행한다. 이는 사용자 전환 명령이며 루트 권한을 얻는다. su postgres
 - Postgres에 로그인 하면, 프롬프트가 postgres@kali:/root$로 변경된다. 이는 애플리케이션,호스트명,사용자를 표현한다.
 - createuser mfs_user -P -> createuser 명령에 -P 옵션을 사용해 사용자명 msf_user를 생성한다. 그러고 나서 사용할 비밀번호를 2번 입력하면 계정이 생성된다.
 - createdb --owner=msf_user hackers_arise_db -> db 이름이 hackers_arise_db인 데이터베이스를 생성하고 msf_user를 위한 권한을 승인한다.
 - exit -> Postgres를 종료한다. 터미널은 다시 msf > 프롬프트를 보여준다.
 - msf >db_connect msf_user:password@127.0.0.1/hackers_arise_db 를 입력하면 msfconsole에서 데이터베이스로 접근할 수 있다.
 - msf >db_status -> 접속을 확인하기 위해서 PostgreSQL 데이터베이스의 상태도 점검할 수 있다.
