## Day 27: Git Revert Some Changes

Task for today is to revert some changes in a Git repository.

```bash
git revert <commit-hash>
```
or, you can use:
```bash
git revert HEAD~1
```

This command creates a new commit that undoes the changes made in the specified commit. If you want to revert multiple commits, you can specify a range of commits.