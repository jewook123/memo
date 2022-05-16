## /bin
리눅스 기본명령어가 등록되어있는 directory

## /sbin
관리 시스템명령어
/bin은 root or user라도 사용할 수 있는 명령어지만
/sbin은 root 사용자 이외에는 보통 환경배스가 걸려있지 않으므로 default로는 사용할 수 없는 명령어

## /usr/bin
bin과 달리 일반 사용자가 사용하는 명령어가 포함되어 있다.

## /usr/sbin
일반 사용자가 사용할 수 있는 관리 명령어

## /usr/local/bin
스스로 설치한 명령어를 사용할 수 있도록 하는 장소
자작 명령어 or 리눅스 저장소 있는 것 이외의 것을 설치했을때 보관할 때 유용하다.

```
echo $PATH
```
위에 명령어를 해보면 아래 우선순위로 환경변수가 잡혀있는것을 볼수 있다.
```
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin
```

### 설치시
```
sudo curl -o /usr/local/bin/kubectl  \
   https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/kubectl
```
```
cURL ref.
https://www.crocus.co.kr/1736
-o --output <file>
https://kibua20.tistory.com/148
```


