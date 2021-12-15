---
title: Git Cheat Sheet
date: 2021-02-25 21:22:42 +05:30
tags: [git, HowTo]
description: A short collection of common git commands and tips.
comments: true
---

##### Git Branch Basics
- Show remote branches `git branch -r`
- Show all branches `git branch -a`
- Create a new branch `git checkout -b [branch_name]`
- Delete a local branch `git branch -d [branch_name]`
- Delete a local branch forcefully `git branch -D [branch_name]`
= Delete [branch_name] from remote (named origin)  `git push origin :[branch_name]` 
- Find common ancestor of two branches `git merge-base [branch_1] [branch_2]

##### Git Merge
Merge [branch_name] to current branch `git merge [branch_name]`
Merge [source_branch] to [target_branch] `git merge [source_branch] [target_branch]`

Sometimes, this doesn't go smoothly and you will have merge conflicts. You will see something like this:
```
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result
```
After resolving merge conflicts:
```
git add <conflicting_file_names>
git commit -m "Your merge comment"
```

##### Git Rebase 
Merge and Rebase solves same problem, but with a slightly different outcome in git hisotry, for more details you can read [this](https://www.atlassian.com/git/tutorials/merging-vs-rebasing){:target="_blank"} and [this](https://stackoverflow.com/questions/16666089/whats-the-difference-between-git-merge-and-git-rebase/16666418#16666418){:target="_blank"}.

Rebase current branch against [source_branch] `git rebase [source_branch]`

Sometimes it is useful to use `git rebase -i` to run rebase interactively and modify the commit history with `fixup` command. I almost always prefer git rebase to git merge as it results in more readable history. But it is important to remember to never ever rebase your public branch such as main, master or develop from a private feature branch. 

also after rebase `git push` fails, so use `git push --force`.


##### Git Reset
Reset everything to specific commit `git reset --hard`. Use with care. 
Reset a single file `git checkout @ -- [filename]`

Use `git revert`, when you need to keep history of reverts.
To revert a commit `git revert [commit_ref]`

Remove tracked file from index `git rm [file_name]`
Remove tracked directory from index `git rm -r  [dir_name]`
Remove files no longer on filesyste:
```
git diff --name-only --diff-filter=D -z | xargs -0 git rm --cached
```

Remove untracked files `git cleani -f`. Use with care, will delete all untracked files. 
Dry run git clean `git clean -n`



##### Git Config 

Configure git user name and email
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Always rebase when you pull 
```
git config --global pull.rebase true
```

Color Output `git config --global color.ui true`

Add Alias ci for commit 
``` 
git config --global alias.ci commit
```




1. [ Merge Vs. Rebase on Bitbucket Tutorial][https://www.atlassian.com/git/tutorials/merging-vs-rebasing]
2. [ Understanding git pull --rebase ][https://gitolite.com/git-pull--rebase]
3. [ Find Common Ancestor for merge ][https://git-scm.com/docs/git-merge-base]
4. [ FIXUP ][https://fle.github.io/git-tip-keep-your-branch-clean-with-fixup-and-autosquash.html]
5. [ Reset on BitBucket ][https://www.atlassian.com/git/tutorials/undoing-changes/git-reset]
6. [ Reset on Stack Overflow ][https://stackoverflow.com/questions/2530060/in-plain-english-what-does-git-reset-do]
