## Day 34: Git Hook

The application development team was working on a git repository /opt/media.git which is cloned under /usr/src/cloudrepos directory present on a server in DC. The team want to setup a hook on this repository, please find below more details: Merge the feature branch into the master branch, but before pushing your changes complete below point. Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release. Finally remember to push your changes.

```bash
cd /usr/src/cloudrepos/media
git checkout master
git merge feature
cd /opt/media.git/hooks
nano post-update
    ```bash
    #!/bin/bash

    cd "$(dirname "$0")/.."

    current_date=$(date +'%Y-%m-%d')
    tag_name="release-$current_date"

    if git rev-parse --verify master >/dev/null 2>&1; then
        git tag -a "$tag_name" master -m "Release for $current_date"
    fi
    ```
chmod +x post-update
cd /usr/src/cloudrepos/media
git push origin master
cd /opt/media.git
git show-ref --tags
```

You should see something like: <commit-hash> refs/tags/release-2025-08-26

On the working copy:
```bash
git fetch --tags
git tag --list
```
You should see something like: release-2025-08-26