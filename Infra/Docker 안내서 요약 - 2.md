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

|옵션|설명|
|-d |detached mode 흔히 말하는 백그라운드 모드|
|-p |호스트와 컨테이너의 포트를 연결 (포워딩)|
|-v |호스트와 컨테이너의 디렉토리를 연결 (마운트)|
|-e|컨테이너 내에서 사용할 환경변수 설정|
|–name|컨테이너 이름 설정|
|–rm|프로세스 종료시 컨테이너 자동 제거|
|-it|-i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
|–link|컨테이너 연결 [컨테이너명:별칭]|
