### tmux란?
원격 서버에서 접속해서 작업시 터미널 화면을 분할해서 사용해야하는 경우, 특히 하나의 터미널 창에서 여러개의 터미널 화면을 분할해서 사용하는 ```Terminal multiplexer```
종류의 소프트웨어를 사용하면 터미널 환경에서의 생산성을 높일수 있다.
원격 연결이 꺼져도 서버가 꺼지지 않는 이상 tmux로 돌려놓은 코드는 다운되지 않는다. 따라서 스크립트 작업시 유용하다.


### 구성요소
- session : 여러 윈도우를 가지는 하나의 세션
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
```

### pane 명령어
```
# 틀 나누기
(ctrl + b) % #좌우로 나누기
(ctrl + b) " #위아래로 나누기

# 틀끼리 이동하기
(ctrl + b) 방향키
(ctrl + b) q
(ctrl + b) o #순서대로 이동

# 틀 삭제
(ctrl + d)

# 틀 사이즈 조정
(ctrl + b) : resize_pane -L 10 #L,R,U,D 입력하면 상하좌우로 조절됨
(ctrl + b) (alt) 방향키

# 단축키 목록
(ctrl + b) ?

```






