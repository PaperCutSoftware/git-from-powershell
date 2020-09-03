# Demonstration of Git at the PowerShell prompt

## Config

```
git config --global --includes --get user.name
git config --global --includes --get core.autocrlf
```

But

```
git config --global --includes --get user.email
```

More on `user.email` later

All these settings are stored in `~/.gitconfig`

## Start a new project

1. Empty directory
2. Add a first file (ex1)
3. `Get-ChildItem`
4. `git status`
5. Create a new Git repo (project)  `git init`
6. `Get-ChildItem`
7. `Get-ChildItem -Force`
8. `git status`
9. `git add <file>`
10. `git status`
11. `git commit`
12. OOps -- `git log`
13. `git config user.email alecclews@gmail.com`
14. `git commit --amend --reset-author`
15. `git log`

**Ta Da!  We have a project under version control**

## Ignore files we don't need to manage

Add directories and files to a magic file called `.gitignore`

For example

1. `Write-Output "/.vscode/" > .gitignore`
2. `git status`

Now `.vscode` is ignored, but `.gitignore` is untracked

3. `git add .gitignore`
4. `git commit -m "Added gitignore file"`
5. `git status`

## Make a change to our project

1. Update our file `ex2`
2. Test
3. Fix
4. `git status`
5. `git diff`
6. `git add .`
7. `git status`
8. `git commit -m "Added comment block`
9. `git log`

## Use `git rm` and `git mv`

**Setup**

1. Add a spurious file `Write-Output "some stuff" > file2`
2. `git add file2`
3. `git commit -m "Silly file"`

**Let's fix it and rename our script**

1. Stage the file delete with `git rm file2`
2. Rename the report file with `git mv report.ps1 papercut-report.ps1`
3. `git status`
4. `git commit`

## See what a previous commit did

`git show HEAD` and `git show HEAD^^`

## Work on a branch

__Create a special version of Epson plugin__

1. Show all our branches `git branch -all`
2. Create a new branch `git checkout -b epson`
3. Do some work (ex3)
4. Test
5. Commit
6. Do some work (ex4)
7. Test
8. Commit
9. `git log` and `gitk --all` show two commits for one piece of work

**Skip on first read**
10. Combine last two commits -- make it clean `git rebase -i HEAD^^`
11. See `git log` `gitk`

## Work on `main` and update (`merge`) changes back to `epson`

1. `git checkout main`
2. add in some changes (ex5)
3. Test
4. add and commit `git ci -am "Add more info and make it pretty"`
5. use `gitk --all` to see branches

##Merge new fancy into Epson version

1. Checkout `epson` branch `git checkout epson`
2. Merge in changes from master `git merge master`
3. Lines clash - see `git status`
4. Manually merge using markers
5. test
6. `git add papercut-report.ps1`
7. `git status`
8. `git commit -m "made epson version pretty"`
9. `gitk --all`
10. Switch back to main `git checkout -`
11. `gitk -all` -- epson has changes from `main`, but `main` does not have changes from `epson`

## Rebase Epson changes on top of main

0. Make sure we are on the `epson` branch `git checkout epson`
1. Make sure the current root working directory is clean, `git status`
2. Now **rebase** current work on top of `main`
3. Fix any problems manually, test, and use `git add <...>`, `git rebase --continue` if required

## Merge Epson changes back into main

1. Check out branch `main`, i.e. `git checkout main`
2. And merge `git merge epson` -- no conflicts because we did all that jazz during the rebase. Note "fast forward"
3. Test
4. Delete merged branch `git branch -f epson`

## Tag a release

1. `git tag rel-1`
2. Move to somewhere else in the commit tree `git checkout HEAD~5`
3. Go back to our tag -- "a rock in a sea of change" `git checkout rel-1`

## Publish our work on GitHub

1. Create an account on GitHib
2. 