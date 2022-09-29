---
layout: post
title: GitHub에서 Fork 하여 pull request 하는법
tags :
    - git
    - github
---

##### 1. pull request를 요청할 repository를 fork
github 에서 Fork 버튼을 클릭하여 pull request 할 repository를 Fork 한다.
  
<img src="/images/posts/7.png">
  
Fork 되면 원본repository 와 내가 fork한 repository를 확인한다.
  
<img src="/images/posts/8.png">
---

##### 2. 내 github repository에서 내가 Fork한 Repository를 Local 저장소로 clone 한다.
```shell
# ex) git clone <Fork 한 Repository URL>
$ git clone https://github.com/gymbombom/hyeongjukim.github.io.git;
```
---

##### 3. local 저장소에 원본 저장소를 추가한다.
내 local 저장소에 원본 repository를 바라보도록 real-master 라는 이름으로 remote저장소를 추가한다.
```shell
$ git remote add real-master https://github.com/hyeongJuKim/hyeongjukim.github.io.git;
```
git remote -v 명령으로 remote저장소 설정을 확인한다.
* origin 은 내가 Fork 한 저장소로 설정되어 있음.
* 위에서 설정한 real-master 라는 이름으로 원본 remote 저장소가 설정되어 있어야 함.
```shell
$ git remote -v
origin	https://github.com/gymbombom/hyeongjukim.github.io.git (fetch)
origin	https://github.com/gymbombom/hyeongjukim.github.io.git (push)
real-master	https://github.com/hyeongJuKim/hyeongjukim.github.io.git (fetch)
real-master	https://github.com/hyeongJuKim/hyeongjukim.github.io.git (push)
```

---

##### 4. Local branch 를 생성
issue1 이라는 이름으로 local branch 를 생성
```shell
$ git branch issue1
```
local branch가 잘 생성되었는지 학인
```shell
$ git branch
  issue1
* master
```
신규 생성한 issue1 branch 로 전환
```shell
$ git checkout issue1
Switched to branch 'issue1'
```
---

##### 5. 수정 후 add, commit, push
변경할 소스를 수정 후 git add 
```shell
$ git add *
```

commit
```shell
$ git commit -m "크론택 -> 크론탭 오타수정"
[issue1 def05c3] 크론택 -> 크론탭 오타수정
 1 file changed, 1 insertion(+), 1 deletion(-)
```

issue1 branch 로 push 한다.
```shell
$ git push origin issue1
Counting objects: 4, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 382 bytes | 382.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
remote:
remote: Create a pull request for 'issue1' on GitHub by visiting:
remote:      https://github.com/gymbombom/hyeongjukim.github.io/pull/new/issue1
remote:
To https://github.com/gymbombom/hyeongjukim.git
```

---

##### 6. pull request 생성
내가 Fork 한 저장소로 이동하여 compare & pull request 버튼을 클릭한다.
   
<img src="/images/posts/9.png">

pull request위치를 확인 후 변경내역을 입력하고 create pull request 를 클릭한다.
  
<img src="/images/posts/10.png">

pull request 요청 내역을 확인하고, 원본 저장소에 write권한이 있는 사용자가 내가 요청한 pull request에 대하여 merge가
될때까지 기다린다.
  
<img src="/images/posts/11.png">

---

##### 7. 원본 저장소에 merge확인
원본 저장소의 write권한이 있는 관리자에 의해서 merge가 완료된 것을 확인한다.
  
<img src="/images/posts/12.png">

---

##### 8. remote branch 삭제
<img src="/images/posts/13.png">

---

##### 9. branch 삭제
master branch 로 전환한다.
```shell
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.
```

local branch 를 삭제한다.
 ```shell
$ git branch -D issue1
Deleted branch issue1 (was def05c3).

```

