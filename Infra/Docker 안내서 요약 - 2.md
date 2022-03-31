## 도커 설치하기 - 리눅스

### 설치 명령어
```curl -fsSL https://get.docker.com/ | sudo sh```
- root권한을 가진 사용자로 위 명령어 실행
   
### sudo 없이 사용하기
```
  sudo usermod -aG docker $USER # 현재 접속중인 사용자에게 권한주기
  sudo usermod -aG docker your-user # your-user 사용자에게 권한주기
```
- docker는 기본적으로 root권한이 필요
- 따라서 현재 사용자를 ```docker 그룹```에 추가해야지만 ```sudo```명령어 없이 사용이 가능하다.
  
### 설치 확인
```docker version```

output
```
Client:
 Version:      1.12.6
 API version:  1.24
 Go version:   go1.6.4
 Git commit:   78d1802
 Built:        Wed Jan 11 00:23:16 2017
 OS/Arch:      darwin/amd64

Server:
 Version:      1.12.6
 API version:  1.24
 Go version:   go1.6.4
 Git commit:   78d1802
 Built:        Wed Jan 11 00:23:16 2017
 OS/Arch:      linux/amd64
```
- server쪽이 정상적으로 되지 않았다면 sudo 권한을 주고 껏킷(or 로그아웃) 해볼것
- docker는 client와 server가 나뉘어짐
  - 실제로 외부와의 통신은 server가 하게 된다. 따라서 proxy 설정을 해주려면 저기에다가 해줘야지 외부와 통신이 가능해진다.
  https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-2/docker-host.png
  - 하지만 그 output을 client가 넘겨서 출력하기 때문에 사용자는 바로 명령을 내리는 느낌을 받게됨

### 실행하기
```docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]```
다음은 자주 사용하는 옵션
| 옵션 | 설명 |
| --- | --- |
| -d | detached mode 흔히 말하는 백그라운드 모드 |
| -p | 호스트와 컨테이너의 포트를 연결 (포워딩) |
| -v | 호스트와 컨테이너의 디렉토리를 연결 (마운트) |
| -e | 컨테이너 내에서 사용할 환경변수 설정 |
| –name | 컨테이너 이름 설정 |
| –rm | 프로세스 종료시 컨테이너 자동 제거 |
| -it | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션 |
| –link | 컨테이너 연결 [컨테이너명:별칭] |
   
### 우분투 설치하기
```docker run ubuntu:16.04```
- proxy 때문에 설치가 안될수 있음
- local에 이미지를 받은적이 없기 때문에 외부 저장소에서 ```pull```한다.
- 컨테이너는 정상적으로 실행됐지만 뭘 하라고 명령어를 전달하지 않았기 때문에 컨테이너는 생성되자마자 종료
```docker run --rm -it ubuntu:16.04 /bin/bash```
- 우분투를 실행후 bash를 실행하라는 명령어
- -rm : 프로세스 종료후 컨테이너 자동 삭제 옵션
- -it : 키보드 입력을 위한 옵션
   
### 레디스 설치하기
```docker run -d -p 1234:6379 redis```
- -d : detached mode (백그라운드 모드)
   - foreground로 실행하면 아무키도 입력할수가 없게 되기 때문에 다른 작업이 어렵다 😅
   - ```ctrl + c```로 탈출 가능
- -p 1234:6379 : 호스트 1234 포트와 컨테이너의 6379 포트를 연결
```telnet localhost 1234```
- telnet 명령어로 테스트 가능
   - telnet 이란?
   >```원격 접속 서비스로서 특정 사용자가 네트워크를 통해 다른 컴퓨터에 연결하여 그 컴퓨터에서 제공하는 서비스를 받을 수 있도록 하는 인터넷 표준 프로토콜```
   
### MySQL5.7 설치하기
```
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql:5.7
```
- -e MYSQL_ALLOW_EMPTY_PASSWORD=true : 환경변수 설정
- -name mysql : 컨테이너에 읽기 어려운 ID 대신 쉬운 이름을 부여
   - 이 옵션을 생략하면 도커가 자동으로 이름 지어줌 ㅎㅎ
   - ```cranky_bouman``` 과 같이 수식어 + 과학자나 개발자 이름 조합으로 랜덤 생성
- ```MySQL Docker hub```페이지에 가면 간단한 사용법과 환경변수에 대한 설명이 있음
   - 이처럼 각 모듈들은 Docker hub에서 인터페이스에 대한 설명을 확인 할 수 있음
   - MYSQL_ALLOW_EMPTY_PASSWORD : 패스워드 없이 root 계정 만들기 on/off
```mysql -h127.0.0.1 -uroot```
- 호스트OS에 mysql 클라이언트가 설치되어있다면 바로 접속 가능
   - ```sudo apt install mysql-client-core-5.7```
- 없다면 ```docker exec -it mysql bash``` 로 컨테이너 안으로 들어가서 확인 가능

### 워드프레스 설치하기
```
mysql -h127.0.0.1 -uroot
create database wp CHARACTER SET utf8;
grant all privileges on wp.* to wp@'%' identified by 'wp';
flush privileges;
quit
```
- MySQL에 wp라는 데이터베이스를 생성하고 wp라는 유저로 접속시 모든 권한을 주는 명령어
```
docker run -d -p 8080:80 \
  --link mysql:mysql \
  -e WORDPRESS_DB_HOST=mysql \
  -e WORDPRESS_DB_NAME=wp \
  -e WORDPRESS_DB_USER=wp \
  -e WORDPRESS_DB_PASSWORD=wp \
  wordpress
```
- 워드프레스의 DB와 연결하는 환경변수를 작성해야한다.
- 앞서 MySQL과 마찬가지로 Docker hub에서 환경변수에 대한 설명이 있다.
   - https://hub.docker.com/_/wordpress
- 


