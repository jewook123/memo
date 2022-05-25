
### 유저 추가
```
sudo su						// 루트 전환
adduser developers
```

※ 유저 사용 용도
```
developers		개발 인원의 ssh 접근시 사용
```

### Docker Repository 등록
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 
```
	
### Docker 설치
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
- 로그아웃 후 적용

### git 프록시 설정
- git config

###  nexus clone


### Docker compose 설치
sudo apt install docker-compose

### dockerfile 내의 인증서 java 루트 고치기

docker-compose up -d 

### 넥서스 배포 이후 admin 설정
docker-compose up -d 
8082

----------------------------------------------------------------------------------------------------------------------------
### nexus 세팅
----------------------------------------------------------------------------------------------------------------------------
### users 추가

### SSL 추가하기
https://help.sonatype.com/repomanager3/nexus-repository-administration/configuring-ssl

### nexus의 SSL 추가?

 - System -> HTTP -> HTTP_PROXY, HTTPS_PROXY 추가
 - SSL Certificates에 repo1.maven.org 추가

jenkins HOME
.gradle/gradle.properties에 저장된 정보 확인

project에서 사용할 정보 확인 
