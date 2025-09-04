## Day 28: Git Cherry-Pick Changes

Task for today is to cherry-pick changes from one branch to another in a Git repository.

```bash
git cherry-pick <commit-hash>
```
This command applies the changes introduced by the specified commit on top of the current branch. If you want to cherry-pick multiple commits, you can specify a range of commits.

```bash
git cherry-pick <start-commit-hash>^..<end-commit-hash>
```
This will apply all commits from the start commit to the end commit, inclusive.