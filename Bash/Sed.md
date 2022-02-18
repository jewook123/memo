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

## 주요 옵션

### -n : 기본 출력 생략

```bash
sed -n '1,3p' /etc/passwd
```

### -f : 스크립트 파일 지정에 사용

### -e : 다중 편집을 가능하게 함

## 주요 커맨드

## 예제로 알아보기

sed -e "s#\S*:##g" -e "s#\s\+#\n#g”

- -e : execute 하는 스크립트 , 보통 두개이상의 표현식을 사용할때 사용
- s : switch
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
