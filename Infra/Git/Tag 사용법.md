## tag의 용도
- 단순히 소스버전을 정하는 용도로 많이들 사용
- 히스토리 
- 빌드시 되돌리기 용이

## tag 사용법
```
# 태그 추가
git tag                                         // 현재 tag 리스트
git tag v2.11.5                                 // 태그 추가하기. 현재 로컬 HEAD에 v2.11.5 태그를 붙인다. **※ 로컬에 먼저 붙이지 않으면 push 할 수 없다.**
git tag -a {tag-name] -m "{tag-message}"        // 태그와 메세지를 같이 추가하기
git push origin v2.11.5 // v2.11.5
Total 0 (delta 0), reused 0 (delta 0)
remote: Processing changes: refs: 1, done
To ssh://~~~
 * [new tag]           v2.11.5 -> v2.11.5
git push --tags                                 // 로컬에 존재하는 모든 tag를 origin(원격)에 올린다

# 태그 삭제
git tag -d v2.11.5                              // 현재 로컬에서 태그명이 v2.11.5인 것을 삭제한다.
git push origin :tags/v2.11.5                   // origin(원격)에 있는 v2.11.5를 삭제한다.

# 오래된 커밋 태그 추가
git tag {tag-name} {commit-id}
git tag v2.11.4 40a2b49                         // 오래된 commit 에 태그 붙이기

# 태그로 체크아웃
git checkout tags/{tag-name} -b {branch}
git checkout tags/v2.11.4                       // 생략시에는 태그이름으로 브랜치 생성

# 태그 목록 가져오기
git fetch --all tags

# 태그 목록 확인
git tag
```
