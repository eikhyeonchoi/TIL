# Pull Request
```
목적: 작업한 코드를 상대방에게 확인하고 merge요청을 하는 행위


상대 repository를 A라고 가정
1. A를 내 git으로 fork
2. git remote add <A주소> <이름:upstream이라 가정> / git remote -v
3. git checkout -b dev(브랜치를파던 master에서 하던 맘대로?)
4. git add/commit/push(push는 해도되고 안해도됨)
5. git checkout master
6. git fetch upstream 변화가 있는지 fetch
7. git merge upstream/master A->로컬로 merge
8. git checkout dev
9. git rebase master
10. git checkout master
11. git merge dev --no-ff || git rebase dev
12. git push origin master
13. pull request 요청
```