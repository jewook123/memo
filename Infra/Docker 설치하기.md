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




