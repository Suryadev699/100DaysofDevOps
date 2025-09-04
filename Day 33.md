## Day 33: Resolve Merge Conflicts

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details: SSH into storage server using user max and password Max_pass123. Under /home/max you will find the story-blog repository. Try to push the changes to the origin repo and fix the issues. The story-index.txt must have titles for all 4 stories. Additionally, there is a typo in The Lion and the Mooose line where Mooose should be Mouse.

```bash
cd /home/max/story-blog/
git status
vi story-index.txt # make the necessary changes
git add story-index.txt
git commit -m "fixed typo"
git pull origin master --rebase
git status #to see the conflicted files
vi story-index.txt #resolve the conflicts
git add story-index.txt
git rebase --continue
git push origin master
```
Once Max has successfully pushed the changes, Sarah can now pull the latest changes to her local repository.

```bash