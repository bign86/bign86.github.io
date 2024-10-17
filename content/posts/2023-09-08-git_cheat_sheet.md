---
title: "Git cheat sheet"
date: 2023-09-08
tags:
  - shell
  - git
categories:
  - git
author_profile: true
draft: false
comments: false
---

Credits for the sources used for this article:

* [A code to remember](https://copdips.com/2019/06/git-cheat-sheet.html)
* [How do I undo the most recent local commits in Git?](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git)

## Alias

Git allows to define aliases in the `.gitignore` file using the syntax

```bash
git config --global alias.<alias_name> <git_command>
```

for globally defined aliases.

A selection of useful aliases:

```bash
git config --global alias.st status
git config --global alias.lga log --graph --decorate --oneline --all
git config --global alias.co checkout
git config --global alias.last log -1 HEAD
git config --global alias.ci commit
git config --global alias.unstage reset HEAD
git config --global alias.ll "log --graph --all --pretty=format:'%C(auto)%h%Creset %an: git config --global %s - %Creset %C(auto)%d%Creset %C(bold black)(%cr)%Creset %C(bold git config --global black)(%ci)%Creset' --no-abbrev-commit"
git config --global alias.sh show
git config --global alias.df diff
git config --global alias.br branch
git config --global alias.cm checkout main
git config --global alias.cd checkout dev
git config --global alias.rum pull --rebase upstream main
git config --global alias.rud pull --rebase upstream dev
git config --global alias.rom pull --rebase origin main
git config --global alias.rod pull --rebase origin dev
```

`~⁄.bashrc`

```bash
alias gitpush='git ci -am $GIT_BRANCH ; git push origin $GIT_BRANCH'
alias gitamendpush='git add . ; git amend ; git push origin $GIT_BRANCH -f'
alias gitrebasemain='git cm ; git rom ; git fetch origin --prune ; git br -d $GIT_BRANCH'
alias gitrebasedev='git cd ; git rod ; git fetch origin --prune ; git br -d $GIT_BRANCH'
```

Restore a file to an old version

```bash
git restore --source [old_commit_hash] [file_name]
```

Restore a deleted branch

```bash
git reflog
git branch [branch_name] [commit_hash_that_preceded_the_delete_commit]
```

Discard changes in working directory

```bash
# discard changes to a file in working directory
git checkout <filename or wildcard>

# discard changes to all files in working directory
git checkout .
# or
git checkout *
```

Untracked files cannot be discarded by checkout.

Discard last commit (completely remove)

```bash
# better to show git log history before using --hard for rollback purpose.
git reset --hard HEAD~
```

We can recover the commit discarded by `--hard` with

```bash
git reflog
git cherry-pick [commit number]
```

if we displayed or saved it before. Whatever you can also use `git reflog` to get the commit number too.

Branch

Force local branch to the same with remote branch

```bash
git reset --hard upstream/master
# or
git checkout -B master origin/master # sometimes this one might not work
```

Get last commit of another local branch

```bash
git cherry-pick another_local_branch
```

Get all commits of another local other_branch

```bash
get rebase another_local_branch
```

Show diff

Show content in staging area

```bash
git diff --cached
```

Show content in the last commit local repository

```bash
git show
git show HEAD
```

Show content in the second last commit in local repository

```bash
git show HEAD~
git show HEAD~1
```

Disable host key checking\\
Sometimes during CICD, we need to use git to do something, if the remote repository is accessed by SSH, the first time when you use git (git clone for example), you need to accept the remote host key. This might be a problem for CICD as it cannot type Y for you as you do in an interactive session. To let git to disable the host key checking or precisely accept automatically the remote host key, you need to add the following line in git config:

```bash
git config --global core.sshcommand 'ssh -i [YouPrivateKeyPath] -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -F /dev/null'
```

You may need to use `git config --system` to set the config at system level.

Moving Git repository content to another repository preserving history

```bash
# https://stackoverflow.com/a/55907198/5095636
# this keeps all commits history and git tags
$ git clone --bare https://github.com/exampleuser/old-repository.git
$ cd old-repository.git
$ git push --mirror https://github.com/exampleuser/new-repository.git
$ cd -
$ rm -rf old-repository.git
```
