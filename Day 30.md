## Day 30: Git hard reset

Task here is to use `git reset --hard` to reset the current branch to a specific commit.

```bash
git log --oneline
```
find the required commit hash and then run:
```bash
git reset --hard <commit-hash>
```
Now push the changes to the remote repository:
```bash
git push origin master --force
``` 
This will reset the current branch to the specified commit and force push the changes to the remote repository.