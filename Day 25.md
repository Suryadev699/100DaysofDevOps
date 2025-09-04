# Day 25: Git Merge Branches

the task is to merge the branch `feature` into the branch `main`.

```bash
# Switch to the main branch
git checkout main
```

```bash
# Merge the feature branch into main
git merge feature
``` 
```bash
# Verify the merge
git log --oneline
```
If any changes to be committed, you can use:
```bash
git commit -m "Merge feature branch into main"
```
