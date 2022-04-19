## ENTRYPOINT와 CMD는 무엇인가?
- 해당 컨테이너가 수행하게될 실행 명령을 정의하는 선언문
- 따라서 Dockerfile의 가장 마지막 부분에 선언
-

## ENTRYPOINT와 CMD 차이점
- 컨테이너 시작시 실행명령에 대한 Default 지정 여부
- ENTRYPOINT
  - 지정한 명령을 수행되도록 지정
- CMD
  - 인자값을 주게 되면 지정한 인자값으로 변경해서 실행

