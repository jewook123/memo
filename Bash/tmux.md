### tmux란?
원격 서버에서 접속해서 작업시 터미널 화면을 분할해서 사용해야하는 경우, 특히 하나의 터미널 창에서 여러개의 터미널 화면을 분할해서 사용하는 ```Terminal multiplexer```
종류의 소프트웨어를 사용하면 터미널 환경에서의 생산성을 높일수 있다.
원격 연결이 꺼져도 서버가 꺼지지 않는 이상 tmux로 돌려놓은 코드는 다운되지 않는다. 따라서 스크립트 작업시 유용하다.


### 구성요소
- session : 제일 큰 실행 단위
- window : 세션의 하위 개념
- pane : 윈도우의 하위 개념

### 명령어
```
# 새로운 세션 생성
tmux new -s (session_name)

# 세션 만들면서 윈도우랑 같이 생성
tmux new -s (session_name) -n (window_name)

# 세션 종료
exit

# 세션 목록
tmux ls

# 세션 중단하기
(ctrl + b) d

# 스크롤하기
ctrl + b + [

# 특정 세션 강제 종료
tmux kill-session -t session_number

```
### window 명령어
```
# 새 윈도우 생성
(ctrl + b) c

# 새 윈도우 이동
(ctrl + b) b (숫자)

# 세션 생성과 함께 윈도우 생성
$ tmux new -s -n

# 윈도우 이름 변경
[Ctrl] + b, ,

# 윈도우 종료
[Ctrl] + b, &
[Ctrl] + d

# 다음 윈도우(Next Window)로 이동
[Ctrl] + b, n

# 이전 윈도우(Previous Window)로 이동
[Ctrl] + b, p

# 마지막 윈도우(Last Window)로 이동
[Ctrl] + b, l

# 특정 윈도우로 이동 (몇 번째 윈도우인지)
[Ctrl] + b, 0-9

# 특정 윈도우로 이동 (이름으로 이동)
[Ctrl] + b, f

# 윈도우 리스트 보기
[Ctrl] + b, w
```

### pane 명령어
```
# 팬 나누기
(ctrl + b) % #좌우로 나누기
(ctrl + b) " #위아래로 나누기

# 팬끼리 이동하기
(ctrl + b) 방향키
(ctrl + b) q
(ctrl + b) o #순서대로 이동

# 팬 삭제
(ctrl + d)

# 팬 사이즈 조정
(ctrl + b) : resize_pane -L 10 #L,R,U,D 입력하면 상하좌우로 조절됨
(ctrl + b) (alt) 방향키

# 단축키 목록
(ctrl + b) ?

# 세로 화면 분할
[Ctrl] + b, %

# 가로 화면 분할
[Ctrl] + b, "

# 팬 이동 - 화면에 나오는 숫자로 이동
[Ctrl] + b, q

# 팬 이동 - 순서대로 이동
[Ctrl] + b, o

# 팬 이동 - 방향키로 이동
[Ctrl] + b, <방향키>

# 팬 삭제
[Ctrl] + d
[Ctrl] + b, x

# 팬 사이즈 조절 - 현재 포커스된 팬 전체화면(한번 더 실행하면 윈상복구)
[Ctrl] + b, z

# 팬 사이즈 조절 [Ctrl] + b 를 누른 후 :
[Ctrl] + b, :
resize-pane -L or -R or -U -D

# 팬 레이아웃 변경 (다양한 레이아웃으로 자동 전환)
[Ctrl] + b, spacebar

```


### etc 명령어
```
# Copy 모드로 들어가기 (스크롤 모드)
[Ctrl] + b, [

# 빠져나오기
[ESC]
q

```





