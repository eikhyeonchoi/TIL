# git revert
git reset은 과거의 commit으로 되돌리지만  
git revert는 현재 commit을 남기면서 과거 commit을 현재 커밋 앞에 붙인다(생성한다)

![revert1](/img/revert1.png)
![revert1](/img/revert2.png)

<br>

```
$ git revert [commit ID]
```