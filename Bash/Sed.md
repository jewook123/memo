# Sed

## 정의

원본 텍스트 변화없이 필터링, 텍스트 변환을 하는 **스트림 편집기 ( stream editor ⇒ sed )**

## 작동방식

**인풋 ⇒ 패턴 스페이스 ⇆ 홀드 스페이스 ⇒ 아웃풋**

- 패턴 스페이스
    - sed가 파일을 라인단위로 읽을때 저장되는 임시 공간
    - 이 공간에서 편집이 일어나기때문에 원본을 건들지 않음
    - 텍스트 1라인→2라인으로 옮겨가게 되면 이공간 역시 2라인으로 변경됨
- 홀드 스페이스
    - 패턴스페이스와 같이 임시 공간이 아니라 조금 더 길게 저장하고 있는 공간
    - 어떤 내용을 홀드 스페이스에 저장한다면 다음행을 읽더라도 재사용할수 있음
- **즉 어떤 인풋을 표현식으로 아웃풋으로 만들어 내가 원하는 값으로 변경할수 있는 효과적인 도구**
    - ex) 3rd party cli를 통해 어떤 값을 읽어오고 필요없는 값을 지우고 내가 원하는 값을 얻고 싶다.
    - 서비스 로그를 통해 가져온 값들중 팀원들에게 의미가 있을만한 것(간헐적 생기는 error..)을 스크랩해서 보내주고 싶다.

### sed --help로 본 사용법
![image](https://user-images.githubusercontent.com/3895536/154674585-a1b3da48-7421-49de-ac6f-f0beed290d51.png)
<br/>
<br/>
맨 위 ```{script-only-if-no-other-script}``` 라는 문구에서 보았듯
```script-file```등으로 대체할 수 있긴하지만 무조건 중간에 ```script```가 들어가야함
```script => stream을 변환하는 식```
<br>
```-e 옵션```을 통해 ```script```를 넣거나 ```-f 옵션```을 통해 ```script-file```을 넣어야 함
<br>

## 주요 옵션

```
# testfile
microsoft
apple
google
naver
tesla
samsung
samsung sds
applepie
```
위 testfile로 아래 예제들을 확인

### n : 자동 출력 생략
```bash
sed "1,3p" testfile 
# script 부분 작성은 아래 주요 커맨드부분에서 언급
# 1 line ~ 3 line 까지의 testfile을 출력하라는 의미의 script
```
하지만 아래처럼 예상과 달리 결과가 다름
패턴 스페이스 부분이 자동 출력되기때문에, script로 작성된 것이 적용되어 ```한번 더``` 출력되서 예상과 달라짐
```
# 예상한 결과
microsoft
apple
google

# 실제 결과
microsoft
microsoft
apple
apple
google
google
naver
tesla
samsung
samsung sds
applepie
```
이를 방지하기위해 ```-n 옵션```을 쓰면 우리가 예상한것처럼 자동출력분을 없앨수 있음
```bash
sed -n "1,3p" testfile # 기본 출력은 제외하고 표현식(1번째라인부터, 3번째라인까지) 출력
```
### e : 다중 편집을 가능하게 함
```script```들을 여러개 넣기 위해서는 -e 옵션이 필요함
```
sed -n -e "1,3p" -e "2p" testfile
```
위처럼 되면 ```첫번째 script```를 통해 나온 결과를  ```두번째 script```로 다시 적용해서 출력함
```
microsoft   # 1번째
apple       # 1번째
apple       # 2번째 적용건
google      # 1번째
```

## 주요 커맨드
위에서 본것처럼 안에 들어가는 ```script```가 중요함
이 과정에서 ```2가지```가 쓰이게됨
```커맨드 & 정규식```

### p : 출력하기
```
$ sed -n "/sam/p"
# 패턴과 일치하는 라인 출력

# 출력결과
samsung
samsung sds

```
### s : 치환하기
```
$ sed -n "s/samsung/lg/p" testfile
# s/패턴 1/패턴 2/
패턴 1인 라인을 찾아 패턴 2로 치환
# p를 통해 출력

# 출력결과
lg
lg sds

```

### d : 삭제하기
```
# 단일형
$ sed "3d" testfile
# 3번째 줄을 삭제하고 나머지 모든 줄 자동 출력 , -n 옵션을 쓰면 안나옴
# 출력 결과
microsoft
apple
naver
tesla
samsung
samsung sds
applepie

# 범위형
$ sed "3,6d" testfile
# 3 ~ 6번째 줄을 삭제하고 나머지 모든 줄 자동 출력 , -n 옵션을 쓰면 안나옴
microsoft
apple
samsung sds
applepie

$ sed '3,$d' testfile
# 3 ~ 마지막 줄까지 삭제하고 나머지 모든 줄 자동 출력 , -n 옵션을 쓰면 안나옴
microsoft
apple

```

## 정규식
정규식 부분은 아래 참조..
https://wrkbr.tistory.com/291


## 예제로 알아보기

```bash
(input stream) | sed -e "s#\S*:##g" -e "s#\s\+#\n#g”
```

- e : execute 하는 스크립트 , 보통 두개이상의 표현식을 사용할때 사용
- s : substituion
    - s\문자열A\문자열B\g
        - 문자열A를 문자열B로 치환함을 위미
        - \ ⇒ #,%로도 사용 가능 , 정규식에 \가 많이 들어가기 때문에 #으로 사용 가능
        - g는 모든 라인에 적용함을 뜻함
    - \S*:는 \S (non space 공백문자가 아닌 것, **정규식**)이 * (반복적으로 앞에 있고, **정규식**) :로 끝나는 문자열을 뜻함
    - 즉 :로 끝나는 문자열을 공백으로 변경함을 뜻함

### 참조

- [https://jhnyang.tistory.com/287](https://jhnyang.tistory.com/287)
- [https://wrkbr.tistory.com/291](https://wrkbr.tistory.com/291)
- [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=minki0127&logNo=220677180665](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=minki0127&logNo=220677180665)
