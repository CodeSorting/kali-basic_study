# 칼리 리눅스 실습 16

 - 주기적으로 실행하기 바라는 스크립트나 태스크인 잡을 갖는다. 시스템을 부팅할 때마다 PostgreSQL을 시작하게 하는 것이 그 예이다.
 - 이 장에서는 cron,crontab 명령으로 시스템에 접속하지 않고도 스크립트를 자동적으로 실행하도록 설정하는 방법을 하급한다.

## 자동 기반 이벤트 또는 잡 스케줄링
 - cron 데몬과 cron 테이블(crontab)은 정기적 작업을 스케줄링하는 가장 유용한 도구다.
 - crond는 백그라운드에서 실행되는 데몬이다. cron데몬은 특정 시간에 실행되는 명령어를 cron 테이블에서 점검한다.
 cron 테이블을 변경해서 태스크 또는 잡을 특정 날짜, 특정 시간에 또는 매주 또는 매달에 정기적으로 실행하도록 스케줄링할 수 있다.
 - 잡을 스케줄링하려면 /etc/crontab에 위치한 cron 테이블 파일에 입력해야한다.
 - cron 테이블은 7개의 필드를 갖는다. 분,시,날짜,월,요일,사용자,명령이다.
 ex) M H DOM MON DOW USER COMMAND에 각각 30 2 * * 1-5 root /root/myscanningscript (DOM,MON에 *는 매일 실행하라는 것이다.)
 - crontab을 수정하려면 crontab -e에서 사용을 원하는 편집기를 선택하거나 vi /etc/crontab에서 새 태스크를 추가하면 된다.
 - 테이블은 17 * * * * root cd / && run-parts --report /etc/cron.hourly처럼 되어 있다.
 - 시스템 관리자 입장에서 보면 시스템이 쉬고 있을 때(일 X) 백업을 보통 수행하고 싶어한다.
 - 보통 새벽 2시이니 일요일 오전으로 넘어가는 새벽 2시에 백업을 수행해보자.
 - 00 2 * * 0 backup /bin/systembackup.sh 라고 crontab에 쓰면 된다.
 이는 00분 2시 모든 날짜 모든 월, 일요일에 backup 사용자로 /bin/systembackup.sh를 실행하는 명령이다.
 - 요일과 관계 없이 매달 15일,30일에만 실행하기 원하면 00 2 15,30 * * backup /bin/systembackup.sh 라고 하면 된다.
 - MySQL 포트를 탐색하는 MySQLscanner.sh를 실행해보자. 00 9 * * * user /usr/share/MySQLscanner.sh를 쓰면 된다.
 만약 트래픽이 적은 주말 새벽 2시에만 스캔하고 싶다면 00 2 * 6-8(6~8월) 0,6 user /usr/share/MySQLscanner.sh를 입력하면 된다.

### crontab 단축어
 - 시간 날짜 월을 입력하는 것 대신 내장 단축어가 있다.
 @yearly,@annually,@monthly,@weekly,@daily,@midnight,@noon,@reboot
 - 만약 매일 자정에 스캐너를 실행하고 싶다면 @midnight user /usr/share/MySQLscanner.sh를 입력하면 된다.

## 부팅 시 잡 실행을 위한 rc스크립트 사용
 - 리눅스 시스템을 시작할 때마다 환경을 설정하기 위해 여러 스크립트가 실행된다. 이들이 rc 스크립트이다.
 - 커널이 초기화되고 모든 모듈이 로드되고 나면, 커널은 init 또는 initrd라 부르는 데몬을 구동한다.
 그러고 나서 이 데몬은 /etc/init.d/rc에서 발견되는 여러 스크립트를 실행한다. 이들 스크립트에서는 리눅스 시스템에서 실행하고자 했던 다양한 서비스들을 구동하는 명령어가 포함된다.

### 리눅스 런레벨
 - 리눅스는 부팅 시 시작되어야 하는 서비스를 가리키는 여러 런레벨이 있다.
 0: 시스템 종료
 1: 단일 사용자/최소 모드
 2~5: 다중 사용자 모드
 6: 시스템 재부팅

### rc.d에 서비스 추가
 - update-rc.d 명령으로 시작시 실행할 서비스를 rc.d 스크립트에 추가할 수 있다.
 - update-rc.d <스크립트 명, 서비스 명> remove|defaults|disable|enable로 이용한다.
 - 예제로 PostgreSQL을 자동으로 실행하도록 해보자. update-rc.d postgresql defaults -> 부팅 시 PostgreSQL을 자동으로 실행한다.

### GUI를 이용한 시작 서비스 추가
 - GUI 기반 도구인 rcconf를 다운로드하면 GUI로 추가할 수 있다.
 - apt-get install rcconf -> rcconf 다운로드
 - rcconf -> rcconf 실행, 이제 GUI 화면이 나오면 사용 가능한 서비스를 스크롤할 수 있고 부팅 시 시작할 서비스를 선택할 수 있다. 선택하고 OK를 누르면 된다.