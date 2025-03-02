# 칼리 리눅스 실습 4

## 소프트웨어 추가 및 제거
 - 리눅스를 퐘한 모든 운영체제의 기본적인 작업은 소프트웨어를 추가하고 제거하는 것이다.
 - 보통 소프트웨어 패키지로 한번에 다운로드 하는 경우가 많다.
 - 소프트웨어를 추가하는 3가지 방법인 apt 패키지 관리자, GUI 기반 설치 관리자, git에 대해 알아보자.

## 소프트웨어 관리를 위한 apt 사용
 - apt(advanced packaging tool)은 데비안 기반 리눅스 배포판에서의 기본 소프트웨어 관리자다.
 - apt-cache search [키워드] : apt 캐시 또는 패키지 이름을 저장하는 위치를 검색한다.
 - apt-get install 패키지이름 : 소프트웨어를 다운로드할 수 있다.(보통 권한이 부족할 수 있으니 sudo를 맨 앞에 붙이자.)
 - apt-get remove 패키지이름 : 소프트웨어를 제거할 때 제거 옵션과 함께 쓰면 된다.
 - apt-get purge 패키지이름 : 패키지와 동시에 구성 파일까지 제거하는 경우
 - apt-get update : 모든 오래된 패키지 업데이트 (최신 상태)
 - apt-get upgrade : 시스템의 모든 패키지를 업그레이드 한다.(sudo 써야함.) (최신 버전)
 - repository : 리눅스에서 패키지를 다운로드하고 설치할 수 있는 공식 서버나 미러 사이트
 - sources.list 파일 : /etc/apt/sources.list에 있으며 리포지토리의 위치(서버 URL)를 저장하는 설정 파일이다.
 - 대부분의 리눅스 뱊판은 리포지터리를 별도의 카테고리로 나눈다. 데비안의 경우를 보자.
    1. main : 지원되는 오픈소스 소프트웨어가 포함되어 있다.
    2. universe : 커뮤니티에서 관리하는 오픈소스 소프트웨어가 포함되어 있다.
    3. multiverse : 저작권 또는 기타 법적 문제로 인해 제한된 소프트웨어가 포함되어 있다.
    4. restricted : 독점 장치 드라이버가 포함되어 있다.
    5. backports : 추후 릴리스 버전의 패키지가 포함되어 있다.
 - 시스템에 문제가 있는 소프트웨어를 다운로드할 가능성 때문에 sources.list에서 실험적이거나 불안정한 저장소 사용을 권장하지 않는다.
 - 완전히 테스트되지 않은 소프트웨어가 시스템을 손상시킬 수 있기 때문이다.
 ex) sources.list에 deb https://ppa.launchpad.net/webupd8team/java/ubuntu trusty main
                    deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu precise main 입력하면 apt-get install oracle-java8-installer 명령으로
                    오라클 자바 8을 설치할 수 있다.

## GUI 기반 설치 관리자
 - 최신 버전 칼리에는 더 이상 GUI 기반 소프트웨어 설치 도구가 포함되어 있지 않지만 apt-get으로 설치할 수 있다.
 - 가장 일반적인 GUI 기반 설치 도구 2가지는 Synaptic,Gdebi이다.
 - apt-get install synaptic -> synaptic이 설치되면 synaptic을 명령줄에 입력하거나 GUI에서는 Settings > Synaptic Package Manager로 들어가 시작할 수 있다.
 - 들어간 다음 find(검색창)에서 패키지를 검색할 수 있다.

## git으로 소프트웨어 설치하기
 - 원하는 소프트웨어가 어떤 저장소에도 없을 때 github에서 공유한 파일을 다운로드해야한다.
 - git clone https://www.github.com/balle/bluediving.git으로 블루 다이빙(블루투스 해킹 침투 테스트 제품군)을 다운로드할 수 있다.
