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