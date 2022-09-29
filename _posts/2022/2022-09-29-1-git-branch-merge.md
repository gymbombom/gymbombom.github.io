---
layout: post
title: Git Branch 생성 및 Merge
tags :
    - git
    - github
---

##### 1. Git Branch 생성
```shell
# git branch <branch명>
$ git branch paging
```
---

##### 2. Git Branch 확인
branch 확인 시 *(별표) 표시된 branch는 현재 선택된 branch를 의미한다.
```shell
$ git branch
* main
  paging
```  
---

##### 3. Git Branch 변경
```shell
# git checkout <branch명>
$ git checkout paging
Switched to branch 'paging'
```
---

##### 4. Branch 간 비교
비교하는 branch간 차이점을 비교할수 있다.
```shell
$ git diff main..paging
diff --git a/post/index.html b/post/index.html
index 2a4d8b7..e5b6bc9 100644
--- a/post/index.html
+++ b/post/index.html
@@ -17,15 +17,13 @@ layout: default
```
---


##### 5. Branch 간 Merge
main branch에 paging branch 의 수정사항을 merge 한다.<br>
먼저, 현재의 branch를 main branch로 변경한다.
```shell
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```
main branch에 병합하고자 하는 branch를 merge 한다. 
```shell
$ git merge paging
Updating 563409c..4a826c9
Fast-forward
 post/index.html | 12 +++++-------
 1 file changed, 5 insertions(+), 7 deletions(-)
```
---
##### 5. Branch 삭제
Merge 가 완료되었으면, branch를 삭제한다.
```shell
$ git branch -d paging
Deleted branch paging (was 4a826c9).
```
