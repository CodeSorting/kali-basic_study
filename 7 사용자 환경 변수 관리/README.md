# 칼리 리눅스 실습 7

## 환경 변수 조회 및 수정
 - 리눅스에서 변수는 2종류가 존재하다. 1. 셸 변수, 2. 환경 변수이다.
 - 환경 변수는 시스템에 내장된 프로세스 전체에서 사용되는 변수이며, 시스템의 모양, 행위, 경험 등의 제어를 사용자에게 연결해준다.
 - 셸 변수는 보통 소문자로 표시되며, 설정된 셸에서만 유용하다.
 - 변수는 '키-값'쌍으로 이루어진 문자열이다. KEY=value,KEY=value1:value2(여러 값을 가질 때)가 그 예시이다.
 - 값 사이에 빈공간이 들어가면 따옴표로 둘러싸야한다.
 - env(environment) 명령 : 터미널의 기본 환경 변수를 조회 가능
 ``` env 명령의 일부
 HOME=/home/kali <- 환경 변수는 HOME,PATH,SHELL 등과 같이 항상 대문자이다.
 LANG=en_US.UTF-8
 ```
 - 사용자 고유의 변수를 생성하기 위해서는 다른 명령어가 필요하다.
 - set | more : 모든 환경 변수(셸 변수, 로컬 변수, 사용자 정의 셸 함수, 명령 알리아스) 조회 -> 다시 돌아가고 싶을 경우 q 이용
 - 특정 변수를 필터링할 수도 있다. set | grep HISTSIZE -> HISTSIZE 환경 변수(히스토리 파일에 저장할 수 있는 명령어의 최대 개수)
 - HISTSIZE=10 -> histsize를 10으로 변경했다. 이제 history를 입력하면 최대 10개까지만 명령어를 기록한다.
 - 그러나 터미널을 닫으면 다시 기본값으로 돌아간다.(영구적X) export를 사용하면 조금 나으나 그래도 영구적이진 않음.
 - export : 현재 환경의 새 값을 새롭게 포크되는 자식 프로세스로 내보낸다. 이는 새 프로세스가 내보내진 변수를 상속받도록 한다.
 - 변수는 문자열이므로 수정하기 전에 텍스트 파일로 변수의 내용을 저장하는 것이 좋다.
 - echo : 터미널에 문자 출력 ex) NAME="Alice" (enter) echo "Hello, $NAME!"
 - echo $HISTSIZE > ~/valueofHISTSIZE.txt -> histsize를 출력하고 valueofHISTSIZE.txt 파일 안에(없으면 만들고) 저장한다.
 - 만약 모든 현재 설정을 갖는 텍스트파일을 생성하고 싶을 때는 set> ~/valueofALL.txt -> valueofALL.txt에 현재 모든 환경 변수 설정 저장.
 - export HISTSIZE : 이를 전체 환경에 적용한다.
 - 그러나 이 조차도 프롬프트를 끄면 사라짐. 설정을 영구적으로 적용하려면 Zsh 설정 파일(~/.zshrc)에 저장해야 함.

## 셸 프롬프트 변경
 - 셸 프롬프트는 유용한 정보를 제공한다. 운용중인 사용자, 현재 작업 중인 디렉터리가 그 예시이다. (username@hostname:current_directory #)
 - 배시 셸을 사용하던 이전 버전은 위와 같으나, zsh로 변경된 지금은 사용자가 선택한 테마에 따라 모양이 다르다. (기본 : (kali@kali)-[~])
 - 루트 사용자로 작업한다면 root@kali:current_directory # 처럼 보인다.
 - PS1은 셸 프롬프트의 모양을 결정하는 환경변수이다. 이는 몇가지 placeholder를 지니는데 \u(현재 사용자의 이름),\h(호스트명),\w(현재 작업 디렉터리 기본 이름) 등이 있다.
 - PS1="HELLO WORLD!"를 입력하면 프롬프트가 이제부터 HELLO WORLD!로 표시해준다. 계속 사용하고 싶으면 export는 해줘야 한다.
 - 윈도우 cmd 프롬프트 처럼 보이는 것을 원하면 C:로 변경하고 \w로 현재 디렉터리를 보이도록 하면 된다.
 - export PS1='C:\w>'를 입력해보면 된다. (zsh 셸은 안되고 BASH 셸을 쓰긴 해야함. zsh는 %~)

## PATH 변경
 - 시스템 환경에서 가장 중요한 변수는 PATH 변수이다. 이는 시스템에서 셸이 grep,ls,echo 같이 입력되는 명령을 어디서 찾아야 될지 제어한다.
 - 대부분의 명령은 sbin 또는 bin 하위 디렉터리(/usr/local/sbin or /usr/local/bin)에 위치한다.
 - 만약 셸이 PATH 변수 내의 디렉터리 중 하나에서 명령을 찾을 수 없다면 command not found 오류를 반환할 것이다. PATH에 있지 않으면 오류가 난다.
 - echo $PATH : PATH 변수가 저장하고 있는 디렉터리를 찾을 수 있다. (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games)
 - 위의 것들은 터미널이 명령을 찾아야 할 디렉터리들이다. 예를 들어 ls를 입력하면 시스템은 ls를 찾기 위해 이 디렉터리들에서 찾아야 함을 알고 있다.
 - 각 디렉터리는 콜론(:)으로 구별된다. PATH에 $기호를 꼭 붙여야한다. 변수 앞에 $를 붙이면 변수의 내용을 조회하는 것이다.
 - PATH 변수에 디렉터리를 추가하고 싶은 경우 PATH=$PATH:/root/newhackingtool -> 원래 PATH 변수 + /root/newhackingtool 디렉터리 추가가 된다.
 - 주의) 너무 많은 디렉터리를 PATH에 추가할 경우 터미널을 느리게 만든다.
 - 주의2) PATH=/root/newhackingtool을 입력할 경우 PATH에 /root/newhackingtool 디렉터리만 남는다.

## 사용자 정의 변수 생성
 - MYNEWVARIABLE="HACKING IS DIFFICULT" 하면 새로운 변수를 생성하고 문자열을 할당한다.
 - unset MYNEWVARIABLE : 사용자 변수 삭제
 