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

- Docker repository 등록   
``` 
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 
```
- Docker 설치
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
- Docker 버전 확인
```
$ sudo docker version
```
- Docker 사용자 그룹 추가
  - usermod : 사용자 속성을 변경하는 명령어
  - -G (—groups) : 새로운 그룹을 말한다.
  - -a (—append) : 다른 그룹에서 삭제 없이 G 옵션에 따른 그룹에 사용자를 추가한다.
  - $USER : 현재 사용자를 가리키는 환경변수
  - 실행 후 우분투를 재기동해야 함.
```
$ sudo usermod -aG docker $USER
```
    

