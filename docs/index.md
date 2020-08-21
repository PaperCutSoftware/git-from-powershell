# Git from the PowerShell Prompt

## Introduction

Most folks in IT (application developers, system administrators, security engineers,...) use and manage text files of various types all day, every day.
For example source code, scripts,  data files of various types (such as config settings stored in JSON or CSV files), application source code and so on.
Keeping track of changes, looking back at old versions and creating special purpose versions is unmanageable without a version control tool.

[Git](https://git-scm.com/) is the world's most popular version control tool. This talk provides a novice introduction to using Git from the PowerShell prompt and no previous version control experience is assumed. As well as using Git locally, we will also look at storing version repositories on the GitHub cloud service.

Because this is an introduction additional resources will be provided to take people further on their Git journey.

Things we'll talk about

- What is the problem Git solves and how?
- A high level overview of Git
- Installing and configuring Git on Windows
- The ten everyday Git commands you need on the PowerShell prompt
- Storing and sharing your files on GitHub
- What to read next

About Alec

Alec is an IT geek who currently works as a Developer Advocate at PaperCut Software in Melbourne, Australia. He's been using computers since the late '70s (an ICL 2904 mainframe) and he was a MS-DOS batch file (and later UNIX shell) wizard. More recently Alec has been learning PowerShell, he always has Windows Terminal open with both a PowerShell and WSL2 bash prompt available. Recently he installed VS Code on his arm64 Chrome OS tablet

## What is the problem Git solves and how?

- It's hard or even impossible to keep track of all our important files, why they were changed, or create new versions for specific purposes. When we work in a team on different changes to a common set of files the complexity quickly becomes unmanageable.
- Version control is the process of recording the history of changes to files after they are modified. Users can go back in time, get old versions and identify where and why changes were introduced. This means that it’s easier to:
    - protect against changes – accidental or otherwise – and undo a "bad" change
    - track down problems and retrofit fixes to previous versions of files
    - support multiple, simultaneous, changes to a common set of project files (parallel development)
    - retrieve an older set of files (if requested by a customer or manager, for example)
- Version Control Systems (VCS) are not just for developers
    - Anyone who manages changes to files
    - People who need to work together
    - Organisations who need to manage content or compliance
- Git provides
    - Each developer with a local repository (repo):
        - To keep a complete history of all the files in our project, the changes that occurred over time
        - The ability to create branches with unique sets of isolated changes
        - Commands to add new changes and recover old versions
    - Git runs on Windows, Mac OS X, & Linux
    - As well as a powerful tool for the individual developer, it provides a powerful model for cooperation in teams and across teams
    - Code sharing sites like [GitLab](https://gitlab.com/), [GitHub](https://github.com/), and [BitBucket](https://bitbucket.org/) provide facilities for developers to co-operate across the Internet
    - Each Git repo can connect and share code with other repos from the same project
    - Technically Git repositories have a peer to peer relationship.
        In practice developers commit to a single upstream repository and
        multiple [workflows](https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows) can be build on top of this model.

## Installing and configuring Git on Windows

- `Install-Module` `git` & `posh-git`
    - `git` the Git binaries
    - `posh-git` provides tab completion, basic prompt customisation
    - [Git Credential Manager for Windows](https://microsoft.github.io/Git-Credential-Manager-for-Windows/) (manual install at the moment) is also recommended. More info on credential managers [here](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)
- Set up three important config settings user name, email address, default init branch and editor. For example
    - `git config —global user.name "Alec Clews"`
    - `git config —global user.email alecclews@gmail.com`
    - `git config --global init.defaultBranch main` (Needs Git 2.28 or above, more info [here](https://blog.papercut.com/renaming-the-git-master-branch/))
    - `git config —global core.editor "code —wait"` (Example for VS Code)
- Your config settings are stored in `$env:USERPROFILE\.gitconfig`
- Want extra fancy? See [https://www.hanselman.com/blog/HowToMakeAPrettyPromptInWindowsTerminalWithPowerlineNerdFontsCascadiaCodeWSLAndOhmyposh.aspx](https://www.hanselman.com/blog/HowToMakeAPrettyPromptInWindowsTerminalWithPowerlineNerdFontsCascadiaCodeWSLAndOhmyposh.aspx)


## A high level overview of Git

- Git is distributed and each repository clone has a (mostly) complete record of all changes
    - c.f. The Subversion VCS (and many others) is a centralised system with a single repo that all developers connect to make changes
- Your local repo database is stored in `.git`, don't worry about it for now
- Repos are replicated (cloned) amongst multiple users, all with their own unique history.
- Git maintaines information about the other repos that it shares changes with in remote tracking branches
- All changes can be shared with other repos as needed, usually to an "upstream"  repo (by convention called origin)
- Git can handle large numbers of files (for example the GNU/Linux kernel source code). However if you have very large binary files then Git (or other general purpose VCS tools) may not be your best choice, but see [Git Large File Storage](https://git-lfs.github.com/).

See also [What is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)

## Everyday Git commands you need on the PowerShell prompt, with examples

The Git command line interface consists of the command `git` followed by a sub-command and the corresponding arguments and options.
There are many sub-commands anm a myriad of options so it can seem a little overwhelming all at one and so we will focus on the basic workflows.
The further information section below provides resources to take you further

### Important (sub)commands

- [init](https://git-scm.com/docs/git-init) or [clone](https://git-scm.com/docs/git-clone)
    - `init` allows you to initliase a new git repo inside a project that is not already under version control e.g.

        `git init <project_dir>`

    - `clone`  clones the complete history of a remote project. You can now work on a running project. For example, let's clone the Git repo for these examples onto our workstation
    - `git clone[https://github.com/alecthegeek/git-from-powershell.git](https://github.com/alecthegeek/git-from-powershell.git)`
- [add](https://git-scm.com/docs/git-add) et al.

    Adding changes to a Git repo is a two stage process. All changes are staged in the index, before they’re committed into the repo.

    ![https://i2.sitepoint.com/graphics/1749-git-index-diag.thumb.png](https://i2.sitepoint.com/graphics/1749-git-index-diag.thumb.png)

    Note that ALL changes, not just new files, need to be added to the Index (a.k.a Staging) before they can be committed

    Files can be renamed or moved with `[git mv](https://git-scm.com/docs/git-mv) ...` and deleted with `[git rm](https://git-scm.com/docs/git-rm) ...`

- commit
- checkout
- pull & fetch
- branch & merge

## Storing and sharing your files on GitHub

## What to read or watch next

* [The Git Parable](https://tom.preston-werner.com/2009/05/19/the-git-parable.html). An introduction to the concepts behind Git

* [Book](https://git-scm.com/book/)

* A series of short videos introducing Git on PowerShell
[![Video Playlist](https://img.youtube.com/vi/WBg9mlpzEYU/0.jpg)](https://www.youtube.com/playlist?list=PLwNoYdA7KMWn0eLRG6lvp2Ir2npoCjRth "A series of short videos introducing Git on PowerShell")


* A nice, but rapid, intro to VCS, Git and GitHub for web projects — but applies to any type of project
[![Git for web developers](https://img.youtube.com/vi/1u2qu-EmIRc/0.jpg)](https://youtu.be/1u2qu-EmIRc?t=463 "Rapid intro to VCS, Git and GitHub for web projects")

* For people who use https, how to avoid keep entering your password [Git - Credential Storage](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)
