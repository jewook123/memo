## 도커 이미지 만들기
![도커 이미지 만들기](https://subicura.com/generated/assets/article_images/2017-02-10-docker-guide-for-beginners-create-image-and-deploy/create-image-1000-93eeb6052.webp)
- 어떤 애플리케이션을 이미지로 만든다면 리눅스만 설치된 컨테이너에 애플리케이션을 설치 그 상태 그대로 이미지로 저장합니다.
- 가상머신의 스냅샷과 비스므리한 방식입니다.
- 샘플 : https://github.com/search?utf8=%E2%9C%93&q=dockerfile%EF%BF%BC
- https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html 
  - 위 예시와는 다르게 Jenkins를 위한 도커라이징( 도커 이미지를 만듦 )을 실습해보자.
- 이전2편에서는 ```image```를 빌드 하는 부분이 없었다.

## commit과 Dockerfile 빌드
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
-  


