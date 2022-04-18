## 도커 이미지 만들기
![도커 이미지 만들기](https://subicura.com/generated/assets/article_images/2017-02-10-docker-guide-for-beginners-create-image-and-deploy/create-image-1000-93eeb6052.webp)
- 어떤 애플리케이션을 이미지로 만든다면 리눅스만 설치된 컨테이너에 애플리케이션을 설치 그 상태 그대로 이미지로 저장합니다.
- 가상머신의 스냅샷과 비스므리한 방식입니다.
- 샘플 : https://github.com/search?utf8=%E2%9C%93&q=dockerfile%EF%BF%BC
- https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html 
  - 위 예시와는 다르게 Jenkins를 위한 도커라이징( 도커 이미지를 만듦 )을 실습해보자.

## 준비물
- 마운트할 호스트의 디렉토리
- Dockerfile (이미지 빌드용 DSLDomain Specific Language 파일)
- 파일

