# 칼리 리눅스 실습 2

## 명령어 정리
 - head : 파일의 시작 부분 첫 10줄만 보여줌. 더 보고 싶으면 head -20 파일명 이면 된다.
 - tail : 파일의 끝 부분 10줄만 보여줌. 그 나머지는 head와 사용법이 동일하다.
 - nl : 파일의 줄번호를 볼 수 있다. nl /etc/snort/snort.lua : 해당 파일의 줄번호로 볼 수 있다.
 - grep : 텍스트 필터링
   ex) cat /etc/snort/snort.lua | grep output -> output 단어를 포함하는 것만 출력함.
 예시들 : nl -ba /etc/snort/snort.conf | grep output # 원하는게 539~544줄일 경우<br>
          tail -n+539 /etc/snort/snort.conf | head -n 6 # 539줄에서 위에서부터 6줄 출력함
  - sed : 명령어 단어나 텍스트 패턴의 출현을 검색하고, 특정 행위를 할 수 있게 해준다.
  cat /etc/snort/snort.conf | grep mysql -> snort.conf에서 mysql라는 단어가 있는 내용을 출력함<br>
  sed s/mysql/MYSQL/g /etc/snort/snort.conf > snort2.conf -> mysql을 MYSQL로 바꾸고 snort2.conf에 저장한다.<br>
  s 명령은 치환을 실행한다. g는 치환을 파일 전체에서 실행하라는 플래그다. 각각은 슬래시로 구분한다.<br>
  sed s/mysql/MYSQL/는 처음 mysql만 변경, sed s/mysql/MYSQL/2는 2번째 나타나는 mysql만 변경한다.
  - more : 한번에 한 페이지만 출력한다. 엔터로 계속 넘길 수 있다.
  - less : more와 비슷하지만 /를 누르고 단어를 입력하고 엔터치면 특정 단어가 있는 곳으로 이동한다. (잘 보니 more도 가능.)
