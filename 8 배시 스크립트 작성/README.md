# 칼리 리눅스 실습 8

## 배시 스크립트에 대하여
 - 보통 루비, 파이썬, 펄 같은 스크립트 언어 중 하나로 스크립트를 작성하기도 한다.
 - 이번 장에서는 간단한 배시 셸 스크립트를 제작한다.
 - 셸은 파일을 조작, 명령, 유틸리티, 프로그램 등을 실행할 수 있도록 하는 사용자와 운영체제 간의 인터페이스다.
 - korn shell, Z shell, C shell, Bourn-again shell 등 다양한 리눅스용 셸을 사용할 수 있다.
 - 배시 셸은 일반적인 명령줄에서 실행할 수 있는 모든 시스템 명령,유틸리티,애플리케이션을 실행할 수 있지만 자체 내장 명령도 포함한다.
 - 보통 vi,vim,emacs,gedit,kate로 셸 스크립트를 작성한다.

## 예시
 - "Hello, World!"를 출력하도록 할 것이다.
 - vi 편집기로 특정 위치에 Helloscript 파일을 만들고 실행한다.
 - 그리고 #! /bin/bash 다음 줄에 echo "Hello, World!"를 입력하고 저장하고 나온다.
 - ls -l로 보면 실행 권한이 없으므로 chmod 755 Helloscript를 입력해 실행 권한을 주고 실행(./Helloscript 입력)하면 잘 실행된다.
 - 변수 및 사용자 입력도 사용이 가능하다.
 - read 변수이름으로 사용이 가능하다. 이 경우에는 사용자의 입력을 받아 변수로 저장한다. 변수 값을 쓰고 싶으면 $변수이름 을 사용하면 된다.
 - 꼭 필요한 것은 아니나 파일 이름 끝을 .sh로 두면 쉘 스크립트라는게 보인다.

## 고급 예제 - 블랙햇 예제 이용
 - 블랙햇 해커는 악의적인 의도를 가진 해커다. 화이트햇 해커는 선의의 해커다. 그레이헷 해커는 양극단을 오간다.
 - nmap(network map)은 시스템이 네트워크에 연결되어 있는지 확인하고 열려 있는 포트를 찾는데 사용된다.
 - 발견된 열린 포트를 확인하면 대상 시스템에 실행 중인 서비스를 추측할 수 있다.
 - nmap [스캔 종류] [대상 IP] [대상 포트(선택적)] ex) nmap -sT 192.168.181.1 -> -sT(TCP 스캔)으로 IP 주소 192.168.181.1 스캔, nmap -sT 192.168.181.1 -p 3306 -> 아까와 동일 + 3306포트를 확인
 - 이번에 할 것은 MySQL의 포트 번호 3306을 이용해 유비쿼터스 온라인 데이터베이스 MySQL에 연결된 시스템을 스캔하는 것이다.
 ```
 -p 3306: MySQL의 기본 포트인 3306번 포트만을 대상으로 스캔한다.
 > /dev/null: 출력 결과를 /dev/null로 보내어 화면에 출력되지 않게함.
 -oG MySQLscan: 결과를 "MySQLscan"이라는 파일에 grep 형식으로 저장한다.
 #! /bin/bash
 # this script is designed to find hosts with MySQL installed
   nmap -sT 192.168.181.0/24 -p 3306 > /dev/null -oG MySQLscan
cat MySQLscan | grep open > MySQLscan2
cat MySQLscan2
 ```
 - 해커 스크립트에 프롬프트 및 변수를 추가해보자.
 ```
 #! /bin/bash
 echo "Enter the starting IP address : "
 read FirstIP
 echo "Enter the last octet of the last IP address : "
 read LastOctetIP
 echo "Enter the port number you want to scan for : "
 read port
 nmap -sT $FirstIP-$LastOctetIP -p $port >/dev/null -oG MySQLscan # first-last로 ip 범위 지정 가능. ex) $FirstIP가 192.168.1.1이고, $LastOctetIP가 192.168.1.10
 cat MySQLscan | grep open > MySQLscan2
 cat MySQLscan2
 
 ```

## 일반적인 기본 제공 배시 명령
 1. : -> 0 또는 참을 반환
 2. . -> 셸 스크립트를 실행
 3. bg -> 백그라운드에서 작업을 수행
 4. break -> 반복 종료
 5. cd -> 디렉터리 변경
 6. continue -> 현재 반복 재개
 7. echo -> 명령 인수 표시
 8. eval -> 인수로 받은 명령을 평가하여 실행
 9. exec -> 새로운 프로세스로 명령어 실행 (현재 셸을 종료하고 새 프로그램을 실행)
 10. exit -> 스크립트나 셸 세션을 종료
 11. export -> 환경 변수를 설정하여 하위 프로세스에 전달
 12. fg -> 백그라운드 작업을 포그라운드로 가져오기
 13. getopts -> 명령줄 옵션을 파싱하는 함수
 14. jobs -> 백그라운드에서 실행 중인 작업 목록 표시
 15. pwd -> 현재 작업 중인 디렉터리의 절대 경로 출력
 16. read -> 사용자 입력을 받아 변수에 저장
 17. readonly -> 변수 값을 읽기 전용으로 설정
 18. set -> 셸 환경 변수를 설정하거나 셸 옵션을 설정
 19. shift -> 위치 매개변수의 위치를 왼쪽으로 이동
 20. test -> 조건식을 평가하여 결과에 따라 참/거짓 반환
 21. [[ -> 복잡한 조건식을 평가하는 명령 (test의 확장)
 22. times -> 프로세스의 실행 시간을 출력
 23. trap -> 시그널을 포착하여 지정된 명령 실행
 24. type -> 명령어의 유형을 출력 (내장 명령어, 외부 명령어 등)
 25. umask -> 파일 생성 시 기본 권한 설정
 26. unset -> 변수나 함수 삭제
 27. wait -> 백그라운드 작업이 종료될 때까지 대기
 ```
 str1="apple"
 str2="banana"

 # 두 문자열이 같은지 확인
 test "$str1" = "$str2" && echo "Strings are equal" || echo "Strings are not equal"

 # 두 문자열이 다른지 확인
 test "$str1" != "$str2" && echo "Strings are different"
 
 num=5

 if test $num -gt 10; then
   echo "$num is greater than 10"
 else
   echo "$num is not greater than 10"
 fi
 ```