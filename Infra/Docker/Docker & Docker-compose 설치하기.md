### 설치 전 확인
- ```sudo``` 권한을 가진 계정으로 진행
  - 없다면 sudoer 추가 ( ※아래 참고 )
> sudoer 추가하는 방법
> 1. ```sudo``` 그룹에 사용자 추가 ( super 유저로 다음 명령문 실행 )  
> ```
> usermod -aG sudo username
> ```   
>    
> 2. ```sudoers```파일에 사용자 추가   
>  ```/etc/sudoers``` 파일에 다음 명령문 추가 
>  ```
>  username ALL=(ALL:ALL) NOPASSWD:ALL
>  ```   
> 위와 같이 명세시 ```sudo```할때마다 비밀번호를 쓰지 않아도 됨   
> ※ ```visudo```명령어를 통해서도 가능  
  - 필요한 경우에만 오래된 버전 삭제하기
    ```
    sudo apt-get remove docker docker-engine docker.io containerd runc
    ```
  - 레포지토리 설정
    ```
    sudo apt-get update
    ``` 
      - 패키지 인덱스 업데이트
    ``` 
    sudo apt-get -y install apt-transport-https ca-certificates curl gnupg lsb-release
    ```
      - 레포지토리 설정을 위한 필요 패키지 설치
  - Docker의 Official GPG Key 등록
    - ※ GPG key : RSA 암호화 기술을 사용한 암호화
    - OH) download시 보안을 위해 사용하는것 같음   
    ```
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```

### Docker 설치

- Docker repository 등록 (필수)   
``` 
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 
```
- Docker 설치 (필수)
```
$ sudo apt-get update
아래 둘중 택 1
$ sudo apt-get install docker.io // version 20
$ sudo apt-get install docker-ce docker-ce-cli containerd.io // version 16

```
- Docker 버전 확인 (필수)
```
$ sudo docker version
```
- Docker 사용자 그룹 추가 (필수)
  - usermod : 사용자 속성을 변경하는 명령어
  - -G (—groups) : 새로운 그룹을 말한다.
  - -a (—append) : 다른 그룹에서 삭제 없이 G 옵션에 따른 그룹에 사용자를 추가한다.
  - $USER : 현재 사용자를 가리키는 환경변수
  - 실행 후 우분투를 재기동해야 함.
```
$ sudo usermod -aG docker $USER
```
### 에러 대응 
- ```sudo docker version```시 Server쪽 에러로그가 뜬다면
  - 위 그룹 추가 후 로그아웃 후 다시 진행

- proxy 설정 오류가 뜬다면
  - docker server (daemon)이 실제로는 외부와 통신을 하게 된다.
  - 그렇기 때문에 이 daemon에 proxy를 설정해줘야한다.
  - ```sudo systemctl show --property=Environment docker``` 
    - 위 명령어를 통해서 Proxy 설정값이 먹혀있는지 확인해볼 수 있다.
    - 안된다면 아래 명령어들을 진행한다.
    - sudo mkdir -p /etc/systemd/system/docker.service.d
    - sudo vi /etc/systemd/system/docker.service.d/http-proxy.conf
      ```
      [Service]
      Environment="HTTP_PROXY=http://proxy.example.com:80"
      Environment="HTTPS_PROXY=https://proxy.example.com:443"
      Environment="NO_PROXY=localhost,127.0.0.1"
      ```
    - sudo systemctl daemon-reload
    - sudo systemctl restart docker
    - sudo systemctl show --property=Environment docker
      - Proxy 설정 된 것 다시 확인하기
    - [Reference : https://blog.naver.com/PostView.nhn?blogId=wideeyed&logNo=222079622746]
- docker pull rate limit이 뜬다면
  - ```anonymous```로는 pull에 한계 ```IP당 100회 / 6시간```를 정해두고 있다.
  - 특히 Proxy 사업장 대부에서 호출을 할 경우, 사업장 내부의 모든 pull이 ```같은 IP```로 동작하기 때문에 엄청 일찍 하지않는다면 pull 하지 못하는 상황이 된다.
    - https://subicura.com/k8s/2021/01/02/docker-hub-pull-limit/
  - 해결책은 개인계정을 사용하면 ```계정당 200회 / 6시간```로 사용할 수 있다.
  - ```docker login -u [ID]```    
### docker-compose 설치
```
// 16
curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-composep-docker 
chmod +x /usr/local/bin/docker-compose

// 20
sudo apt install docker-compose

# test
docker-compose version
```
