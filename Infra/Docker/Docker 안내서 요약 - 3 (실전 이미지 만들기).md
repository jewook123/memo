## 도커 이미지 만들기
![도커 이미지 만들기](https://subicura.com/generated/assets/article_images/2017-02-10-docker-guide-for-beginners-create-image-and-deploy/create-image-1000-93eeb6052.webp)
- 어떤 애플리케이션을 이미지로 만든다면 리눅스만 설치된 컨테이너에 애플리케이션을 설치 그 상태 그대로 이미지로 저장합니다.
- 가상머신의 스냅샷과 비스므리한 방식입니다.
- 샘플 : https://github.com/search?utf8=%E2%9C%93&q=dockerfile%EF%BF%BC
- https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html 
  - 위 예시와는 다르게 Jenkins를 위한 도커라이징( 도커 이미지를 만듦 )을 실습해보자.
- 이전2편에서는 ```image```를 빌드 하는 부분이 없었다.

## 도커 이미지 기초
- 도커의 이미지는 가상머신 이미지와는 다름
- 도커의 이미지는 순수한 파일들의 집합
  - 메모리 정보 OR 독자적인 형식으로 저장해둔 시스템의 정보 X
  - 이미지에 대한 메타데이터 O
- ```LXC```나 ```systemd-nspwn``` 과 같은 초기 컨테이너 도구들은 ```chroot on steroids``` 라는 별명을 가짐
  - 스테로이드 맞은 chroot
  - chroot를 사용하면 실제 루트가 아닌 다른 디렉토리를 루트로 바라보는 프로세스를 실행하는게 가능
  - 즉 ```root를 바꾼다```는 개념
- 리눅스의 흥미로운 특징
  -  우분투 배포판 위에 아마존 리눅스배포판의 파일을 준비하고 chroot로 프로세스의 루트를 아마존 리눅스배포판 디렉토리로 지정하면, 해당 프로세스는 아마존 리눅스의 프로세스 처럼 동작한다.
  -  즉 chroot만으로는 완벽하지 않지만 도커와 같은 프로세스 격리 기술이 컨테이너 런타임을 사용하면 완벽히 다른 배포판으로 동작함
- 도커에서 제공하는 베이스 이미지
  - ```우분투```, ```데비안```, ```레드햇```, ```오픈수세```, ```젠투```, ```아치```, ```아마존 리눅스``` 등이 있으며 경량환경에서 애용되는 ```알파인```도 있음
  - 사용자들은 일반적으로는 이러한 리눅스 배포판 이미지를 베이스로 삼아 커스텀 이미지를 만듦
```
18.04, bionic-20201119, bionic
20.04, focal-20201106, focal, latest
20.10, groovy-20201125.2, groovy, rolling
21.04, hirsute-20201119, hirsute, devel
14.04, trusty-20191217, trusty
16.04, xenial-20201030, xenial
사용예 : ubuntu:18.04
```
## 컨테이너의 이해
만들어볼 이미지는 ```git```이 설치된 ```ubuntu:18.04``` 이미지이다.
- 사용자용 ubuntu는 여러가지 애플리케이션이 설치되어있지만 
- 도커에서 제공 ubuntu는 최소한의 이미지를 사용해 63.3 MB 밖에 되지 않는다.
- ```docker run -it ubuntu:18.04 git --version``` 를 해보면 git이 없는것을 알수 있다.
- ```docker run -it ubuntu:18.04 bash```를 통해 컨테이너를 하나 만들고 git을 설치해보자
  - apt update
  - apt install -y git
  - git --version
  - exit를 명령어로 컨테이너에서 나오자
- ```docker run -it ubuntu:18.04 bash```로 다른 컨테이너를 실행해 git명령어를 쳐보면?? 
  - git은 또 없다고 나온다.   
![컨테이너 비교](https://d2uleea4buiacg.cloudfront.net/files/b12/b12f0c3b35afb6bc835f16437c163b4e9aa789fb2e839faf888ccdf563e3419c.m.png)
- 위에서 apt update, apte install -y git을 한것은 첫번째 컨테이너의 파일시스템만을 건들이게 되는것이기때문에
- 두번째 컨테이너에 아무런 영향이 없는것이다.

## docker commit 명령어로 이미지 만들기
```
# 첫번째 터미널에서 실행
docker run -it ubuntu:18.04 bash
877c9dd67d76# 

# 다른 터미널에서 접속해 확인
docker diff 877c9dd67d76
```
현재 근본이 되는 이미지와 컨테이너(877c9dd67d76) 상의 변경사항이 없기때문에 아무것도 안나오는게 ```정상```이다.

```
# 첫번째 터미널에서 실행
877c9dd67d76# touch hello world
877c9dd67d76# ls
bin  boot  dev  etc  hello  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  world

# 다른 터미널에서 접속해 확인
docker diff 877c9dd67d76
A /world
A /hello
```
- 두개의 파일이 추가되었다는 표시가 나옴
- 위 예가 커스텀 이미지를 만드는 가장 기본이 되는 원리
- 이미지는 달라지지 않음
- hello와 world를 추가한 위 예제를 새로운 커스텀 이미지로 만들수 있다.
- 이 때 사용하는 컨테이너의 파일 공간을 이미지로 저장하는 명령어가 바로 ```commit```이다.
- ```ubuntu:hello-world``` 이미지를 만들어보자
```
# 아래 명령어를 통해서 커스텀 이미지를 생성한다.
docker commit 877c9dd67d76 ubuntu:hello-world

# 만들어진 docker 이미지를 확인할 수 있다.
docker images | grep hello-world

# 위 이미지로 하나 만들어보자!
docker run -it ubuntu:hello-world

# diff를 해보면??
docker diff adbcf3be3d09

# 아무것도 출력되지 않는다.
# 처음부터 /hello, /world 파일이 존재했기 때문에!

```
그러면 이제 git으로 돌아와보자
```
# 첫번째 터미널
docker run -it ubuntu:18.04 bash
5414b8a0279f# apt update
5414b8a0279f# apt install -y git

# 두번째 터미널
# 이렇게 하면 패키지 매니저가 파일공간에 일으킨 변화를 확인할수 있다.
docker diff 5414b8a0279f
# wc -l 은 변경된 파일들 갯수를 출력하게 한다.
docker diff 5414b8a0279f | wc -l

# git을 가진 ubuntu라는 의미에서 tag를 달고 이미지로 커밋한다.
docker commit 5414b8a0279f ubuntu:git
```
