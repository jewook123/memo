### 설치 전 확인
- ```sudo``` 권한을 가진 계정으로 진행
  - 없다면 sudoer 추가 ( ※아래 참고 )
> sudoer 추가하는 방법
> 1. ```sudo``` 그룹에 사용자 추가   
> super 유저로 다음 명령문 실행   
> ```usermod -aG sudo username```   
>    
> 2. ```sudoers```파일에 사용자 추가   
>  ```/etc/sudoers``` 파일에 다음 명령문 추가 ```username ALL=(ALL:ALL) NOPASSWD:ALL```   
> 위와 같이 명세시 ```sudo```할때마다 비밀번호를 쓰지 않아도 됨   
> ※ ```visudo```명령어를 통해서도 가능  
  - 필요한 경우에만 오래된 버전 삭제하기
    - ```sudo apt-get remove docker docker-engine docker.io containerd runc```
  - 레포지토리 설정
    - ```sudo apt-get update``` 
      - 패키지 인덱스 업데이트
    - ``` sudo apt-get -y install apt-transport-https ca-certificates curl gnupg lsb-release```
      - 레포지토리 설정을 위한 필요 패키지 설치
  - Docker의 Official GPG Key 등록
    - ※ GPG key : RSA 암호화 기술을 사용한 암호화

