# Demostration of Git at the PowerShell prompt

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
3. `ls`
4. `git status`
5. Create a new Git repo (project)  `git init`
6. `ls`
7. `ls -Force`
8. `git status`
9. `git add <file>`
10. `git status`
11. `git commit`
12. OOps -- `git log`
13. `git config user.email alecclews@gmail.com`
14. `git commit --amend --reset-author`
15. `git log`
    
**Ta Da!  We have a project under version control**

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

# Use `git rm` and `git mv`

**Setup**

1. Add a spurious file `echo "some stuff" > file2`
2. `git add file2`
3. `git commit -m "Silly file"`

**Let's fix it and rename our script**

1. Stage the file delete with `git rm file2`
2. Rename the report file with `git mv report.ps1 papercut-report.ps1`
3. `git status`
4. `git commit`
5. `git show HEAD` and `git show HEAD^^`

## Work on a branch

**Create a special version of Epson plugin

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

##Ignore files that are not needed

0. Make sure we are on the `epson` branch `git checkout epson`
1. `git status`, see the report file that is not needed in VCS
2. `echo "report1.html" > .gitignore`
3. `git status`
4. We are now ignoring the report file, but we need to commit the ignore list
5. `git add .gitignore && git commit -m "Add ignore list"`
6. Expect to see `.gitignore` files
7. `git status`

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


## Publish our work on GitHub

