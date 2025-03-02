# 칼리 리눅스 실습 11

## 로그
 - 리눅스 사용자라면 로그 파일의 사용법을 아는 것이 중요하다.
 - 로그 파일은 운영체제,애플리케이션이 실행하는 동안 발생한 모든 오류, 보안 경고 등 여러 이벤트 정보를 저장한다.
 - 로그 파일은 대상(해커 등)의 활동 및 정체성을 추적한다.

## rsyslog 로깅 데몬
 - 리눅스는 syslogd라는 데몬을 이용해서 이벤트를 자동으로 기록한다.
 - syslog에는 다양한 변형이 존재한다. rsyslog,syslog-ng를 포함한 여러 로깅이 있다. (debian은 rsyslog를 사용)
 - locate rsyslog -> rsyslog와 관련된 파일을 모두 보여준다.
 - vi /etc/rsyslog.conf를 보다보면 환경설정 파일인 rsyslog.conf에 사용법에 대해 잘 알 수 있다.
 - 55줄에 Rules section에 가보면 리눅스 시스템이 무엇을 자동으로 기록할지에 대한 규칙을 설정할 수 있다.
 ```
 Rules
 # facility.priority action -> facility 키워드는 메시지를 기록할 프로그램을 가리킨다. priority 키워드는 해당 프로그램을 위해 어떤 종류의 메시지를 기록할 것인지 정한다.
 action은 로그가 보내질 위치를 참조한다.
 auth,authpriv : 보안/인증 메시지
 cron : 시간 데몬
 daemon : 기타 데몬
 kern : 커널 메시지
 lpr : 프린트 시스템
 mail : 메일 시스템
 user : 일반 사용자 수준 메시지
 auth,authpriv.*       /var/log/auth.log
 *.*;auth,authpriv.none    -/var/log/syslog
 #cron.*               -/var/log/cron.log
 kern.*                -/var/log/kern.log
 lpr.*                 -/var/log/lpr.log 
 mail.*                -/var/log/mail.log
 user.*                -/var/log/user.log
 mail.info             -/var/log/mail.info
 ....
 
 ```

 ### 로그 수준
 - 로그 수준 (Severity Levels) : priority에 해당
로그 메시지에는 여러 가지 수준이 있으며, 로그가 어느 수준에서 발생하는지에 따라 필터링할 수 있다. 로그 수준은 다음과 같다(warn,error,panic은 더이상 사용을 안한다.)
emerg: 긴급, 시스템이 사용할 수 없는 상태
alert: 경고, 즉각적으로 해결해야 하는 문제
crit: 심각한 오류
err: 오류
warning: 경고
notice: 정상적인, 그러나 주의가 필요한 메시지
info: 정보
debug: 디버깅 메시지

### 예시
예시: kern(커널 로그) 메시지를 /var/log/kern.log에 기록
kern.*                        /var/log/kern.log

예시: error(오류) 메시지를 /var/log/errors.log에 기록
*.err                         /var/log/errors.log

예시: info(정보) 메시지를 /var/log/messages에 기록
*.info                        /var/log/messages

예시: emerg(긴급) 우선순위를 갖는 모든 이벤트를 로그인한 모든 사용자에게 기록한다.
*.emerg :omusmsg:*

## logrotate를 통한 로그 자동 정리
 - 로그 파일은 메모리를 차지해서 주기적으로 삭제해줘야 한다.
 - 로그 순환(주기적으로 다른 위치로 이동(아카이브)하는 절차)을 이용한다.
 - 시스템은 이미 logrotate 유틸리티를 차용한 cron 작업을 통해 로그 파일을 순환시킨다.
 - vi /etc/logrotate.conf를 들어가보면 weekly(순환 시간 단위 설정(1주)),rotate 4(4번(4주)마다 순환),compress(압축(선택사항))등이 있다.
 - man logrotate 페이지에 로그를 다뤄야 하는 방법을 변경하는 변수를 배우기 위한 자원이 있다.
 - ls /var/log/auth.log.* -> /var/log/auth.log.1~4까지 있다. 로그 파일을 4주마다 순환하고 4개의 백업본을 유지하기 때문이다.

## 은신 상태 유지
 - 리눅스 시스템을 취하고 탐지 확률을 낮추기 위해 로그 기록을 비활성화하고, 침입 흔적을 지우는게 좋다.
 - 흔적 삭제를 위해 로그 파일을 파쇄(shred)한다.
 - shred --help를 보면 많은 옵션이 있지만 shred <FILE>이 기본적인 사용 방법이다.
 - f 옵션은 파일의 권한 변경이 필요한 경우 덮어쓰기를 할 수 있게 파일의 권한을 변경하고, n 옵션은 덮어쓸 횟수를 선택할 수 있다.
 ex) shred -f -n 10 /var/log/auth.log.* -> auth로그를 지우기 위해 권한을 얻어야하고(-f), 10번 삭제하고 덮어쓴다.
 - 파쇄하고 vi /var/log/auth.log.1을 보면 알아볼 수 없는 내용이 있다. (파쇄되어서 못 알아봄.)
 - 로깅을 비활성화하는 방법도 있다.
 - service 명령인데 service [서비스명] start|stop|restart가 있다.
 ex) service rsyslog stop -> 리눅스의 서비스가 다시 시작되기 전까지 rsyslog(로그 파일 생성)을 멈춘다.
