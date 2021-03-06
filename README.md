# Git cheatsheet

## Switch between branches

`git checkout <branch-name>`

## Get remote branch locally

`git checkout -b <repo-name> origin/<repo-name>`

## Create branch

* Long way:

 `git branch <branch_name>`

 `git checkout <branch_name>`

* Short way:

 `git checkout -b <branch_name>`

## Rename branch

`git branch -m <oldname> <newname>`

## Change upstream

`git push --set-upstream origin <branch_name>`

## Push new branch to remote

`git push --set-upstream origin <branch_name>`

## Remove branch

* Local:

 `git branch -d <branch-name>`

 But first checkout to some other branch. You cannot remove branch you’re on.

* Remote:

 `git push origin :<branch-name>`

 Looks strange, so to picture this: you’re pushing `<nothing>:<branch-name>`.

* Cleaning information on remote branches

`git remote prune origin`

`git gc --prune=now`
 
* Cleaning all branches aside from master (for badasses only)

`git branch | grep -v "master" | xargs git branch -D`

## Stashing

Comes handy if you want to change branch without committing current stuff.

* To stash:

 `git stash`

* To unstash:

 `git stash apply`

* To list:

 `git stash list`

## Listing names of new files

`git diff --name-only --diff-filter=A master <branch-name>`

## Creating tag

`git tag -a <version, i.e. 1.0.0> -m '<description'`

or shorter,

`git tag <version>`

## Pushing tag

`git push --tags`

## Deleting local tag

`git tag -d <tag>`

## Deleting remote tag

`git push origin :refs/tags/<tag>`

## Changing commit message

`git commit -amend`

But only if commit hasn’t been pushed to repo. Otherwise it might cause problems.

## Resetting branch

When stuff is committed but not pushed.

`git reset --hard origin/master`

## Moving commits to other branch

Scenario: you committed to a branch `master` instead of your `feature/magical-branch-name` but did not yet push. What now?

(You are on master branch.)

```
git checkout feature/<branch-name>
git merge master
git checkout master
git reset --hard origin/master
```

Stuff from `master` is now in feature branch, and `master` itself was resetted to its origin state. As nothing was pushed, nothing happened.

Instead of `master`, of course, you might use `some/other-branch`.

## Restoring deleted local file

First, don’t panic, the data is still there.

Run:

`git reflog`

Then choose last (short) hash with your branch. Next,

`git checkout <hash>`

And then,

`git checkout -b <new-branch-name>`

## Fetching pull request locally

`git fetch origin pull/<pull-req-id>/head:<local-branch-name>`

If pull request is not a branch, this comes handy.

## Credentials

* To remember credentials by deamon:

 `git config --global credential.helper wincred`

* To clear remembering credentials:

 `git config --global --unset credential.helper`

## Removing file from tracking

* If file was committed once but we want to remove it from tracking, the following command will do:

 `git update-index --assume-unchanged <file>`

* To revert it:

 `git update-index --no-assume-unchanged <file>`

## Getting rid of `.idea`

If someone committed and pushed `.idea` folder to the repo, it will become problematic. This is the solution:

```
mv .idea ../.idea_backup
rm .idea # in case you forgot to close your IDE
git add .idea
git commit -m "Remove .idea from repo"
mv ../.idea_backup .idea
```

After than make sure to ignore `.idea` in your `.gitignore`.

## warning: LF will be replaced by CRLF

If some application (for instance, LessJS compiler) messes with line endings, git throws a warning:

>warning: LF will be replaced by CRLF in {file}

To get rid of it, one has to open git config file and add this setting:

`autocrlf = true`
