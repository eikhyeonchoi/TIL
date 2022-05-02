# git reset
과거 commit으로 되돌림

![reset1](/img/reset1.png)
![reset2](/img/reset2.png)

<br>

```
$ git reset --soft [commit ID]
```
commit된 파일들을 staging area로 돌림(commit 하기 전 상태로)

<br>

```
# default option
$ git reset --mixed [commit ID]
```
commit된 파일들을 working directory로 돌림(add 하기 전 상태로)

<br>

```
$ git reset --hard [commit ID]
```
commit된 파일들 중 tracked 파일들을 working directory에서 삭제

<br>

```
$ git reset HEAD~N
```
현재로부터 N만큼의 커밋이 취소

<br>

```
$ git reset HEAD^
```
가장 최근 커밋 취소