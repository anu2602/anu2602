---
title: Git Cheat Sheet
date: 2021-02-25 21:22:42 +05:30
tags: [git, HowTo, CheatSheet]
description: A short collection of common git commands and tips.
image: "/git-cheat-sheet/git-cheat-sheet.jpg"
comments: true

---
<figure>
<img src="git-cheat-sheet.jpg" alt="Git Cheat Sheet">
</figure>

##### Git Branching Basics
- Show remote branches `git branch -r`
- Show all branches `git branch -a`
- Create a new branch `git checkout -b [branch_name]`
- Delete a local branch `git branch -d [branch_name]`
- Delete a local branch forcefully `git branch -D [branch_name]`
- Delete branch from remote (named origin)  `git push origin :[branch_name]` 
- Find common ancestor of two branches `git merge-base [branch_1] [branch_2]

##### Git Merge
- Merge [branch_name] to current branch `git merge [branch_name]`
- Merge [source_branch] to [target_branch] `git merge [source_branch] [target_branch]`

Sometimes, this doesn't go smoothly merge conflicts will show up. 
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result
```
Resolve conflicts and after resolving merge conflicts:
```
git add <conflicting_file_names>
git commit -m "Your merge comment"
git push
```

##### Git Rebase 
Merge and Rebase solve same problems, but with a slightly different outcome in git hisotry; For more details recommed reading [this](https://www.atlassian.com/git/tutorials/merging-vs-rebasing){:target="_blank"} and [this](https://stackoverflow.com/questions/16666089/whats-the-difference-between-git-merge-and-git-rebase/16666418#16666418){:target="_blank"}.

Rebase current branch against [source_branch] `git rebase [source_branch]`

Sometimes it is useful to use `git rebase -i` to run rebase interactively and modify the commit history with `fixup` command. I almost always prefer `git rebase` to `git merge` as it results in more readable history. But it is important to remember to never ever rebase your public branch such as main, master or develop against a private feature branch. 

also after rebase `git push` fails, so use `git push --force`.


##### Git Reset
- Reset everything to specific commit `git reset --hard` ; Use with care
- Reset a single file 
```
git checkout @ -- [filename]
```

- Use `git revert`, when you need to keep history of reverts
- Revert a commit `git revert [commit_ref]`

- Remove tracked file from index `git rm [file_name]`
- Remove tracked directory from index `git rm -r  [dir_name]`
- Remove files no longer on filesystem:
```
git diff --name-only --diff-filter=D -z | xargs -0 git rm --cached
```

- Remove untracked files `git cleani -f`. Use with care, will delete all untracked files. 
- Dry run git clean `git clean -n`


##### Git Config 

Configure git user name and email
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Always rebase on pull (default is merge)
```
git config --global pull.rebase true
```

Colorful Git 
```
git config --global color.ui true
git config --global color.diff true
```

Add Alias ci for commit 
``` 
git config --global alias.ci commit
```

### Reference
1. [ Merge Vs. Rebase on Bitbucket Tutorial](https://www.atlassian.com/git/tutorials/merging-vs-rebasing){:target="_blank"}
2. [ Understanding git pull --rebase ](https://gitolite.com/git-pull--rebase){:target="_blank"}
3. [ Find Common Ancestor for merge ](https://git-scm.com/docs/git-merge-base){:target="_blank"}
4. [ FIXUP ](https://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html){:target="_blank"}
5. [ Reset on BitBucket ](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset){:target="_blank"}
6. [ Reset on Stack Overflow ](https://stackoverflow.com/questions/2530060/in-plain-english-what-does-git-reset-do){:target="_blank"}
