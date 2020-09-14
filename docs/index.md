<!-- markdownlint-disable MD033 -->

# Git from the PowerShell Prompt

These notes support a demonstration and talk that introduces PowerShell users to Git
and version control concepts.
An hour talk is not very long and these notes are brief, so links to more complete material are provided.

Note: This content is written in British English

## Introduction

Most folks in IT (application developers, system administrators, security engineers,...) create, and manage, text files of various types all day, every day.
For example: scripts; data files of various types (such as config settings stored in JSON or CSV files); application source code and so on.
Keeping track of changes, looking back at old versions and creating special purpose versions is unmanageable without a version control tool.

[Git](https://git-scm.com/) is the world's most popular version control tool. This talk provides a novice introduction to using Git from the PowerShell prompt and no previous version control experience is assumed. As well as using Git locally, we will also look at storing version repositories on the GitHub cloud service.

Because this is an introduction additional resources will be provided to take people further on their Git journey.

Things we'll talk about

- What are the problems Version Control Software (VCS) solves and how?
- A high level overview of Git
- Installing and configuring Git on Windows
- The ~~seven~~ first nine everyday Git commands you need on the PowerShell prompt
- Storing and sharing your files on GitHub
- What to read next

In the following explanations we will talk about developers and that means **you**, because you write PowerShell scripts...

> If you write code you're a dev
>
> --<cite>[Thomas Rayner](https://thomasrayner.ca/)

About Alec

[Alec](https://github.com/alecthegeek/) is an IT geek who currently works as a Developer Advocate at [PaperCut Software](https://www.papercut.com/) in [Melbourne](https://en.wikipedia.org/wiki/Melbourne), Australia. He's been using computers since the late '70s (an ICL 2904 mainframe) and he was a MS-DOS batch file (and later UNIX shell) wizard. More recently Alec has been learning PowerShell, he always has Windows Terminal open with both a PowerShell and WSL2 Bash prompt available. Recently he installed VS Code on his ARM64 Chrome OS tablet

<!-- markdownlint-disable MD026 -->
## What are the problems VCS solves and how?
<!-- markdownlint-enable MD026 -->

- It's hard, or even impossible, to keep track of all our important project files, why they were changed, or create new versions for specific purposes. When we work in a team on different changes to a common set of files, the complexity quickly becomes unmanageable.

- [Version Control](https://en.wikipedia.org/wiki/Version_control) is the process of recording the history of changes to files. Users can go back in time, retrieve old versions and identify where and why changes were introduced. This means that it’s easier to:
  - protect against unnecessary changes and undo a "bad" change
  - track down problems and retrofit fixes to previous versions of files
  - support multiple, simultaneous, changes to a common set of project files (parallel development) and release different versions
  of the same product from a common code base
  - retrieve an older set of files (if requested by a customer or manager, for example)

- Version Control Systems (VCS) are not just for developers
  - Anyone who manages changes to files
  - People who need to work together
  - Organisations who need to manage content or satisfy compliance requirements

- All modern version control systems provides developers with some form of database that records the changes to files
as a set of revisions or snap shots in time
  - The VCS database is often referred  to as the repository (**repo**)
  - Adding a new collection of changes (for instance to fix a specific issue) is called a **commit**
  - Developers need to provide sensible information about the commit for VCS to be effective
  - Obtaining the contents of a specific commit from the repo is referred to a **checkout**

- As well as a powerful tool for the individual developer, it provides powerful mechanisms for cooperation
within teams and between teams

## Installing and configuring Git on Windows

- Personally I prefer to install via [Chocolaty](https://chocolatey.org/)
  - `choco install git poshgit`
    - `git`: the Git package for Windows. I prefer to use Chocolaty or download the [installer](https://git-scm.com/download/win).
    - [`posh-git`](https://github.com/dahlbyk/posh-git/blob/master/README.md): provides tab completion and
      basic prompt customisation. Supports Windows PowerShell 5.x or PowerShell Core 6+ on all platforms

- Can also use PowerShell Module install, e.g. `Install-Script Install-Git ; Install-Git.ps1 ; Install-Module posh-git ; Import-Module posh-git`. However the Git Module does not present the standard CLI experience.

- Also recommended,
  [Git Credential Manager for Windows](https://microsoft.github.io/Git-Credential-Manager-for-Windows/) (manual install at the moment).
  More info on credential managers [here](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)

- Set up some important config settings user name, email address, default init branch and editor. For example
  - `git config --global user.name "Alec Clews"`
  - `git config --global init.defaultBranch main` (Needs Git 2.28 or above, more info [here](https://blog.papercut.com/renaming-the-git-master-branch/))
  - `git config --global core.editor "code --wait"` ([VS Code](https://code.visualstudio.com/) example)
  - `git config --global core.autocrlf input` so that you [play nice with UNIX style line endings](https://code.visualstudio.com/docs/remote/troubleshooting#_resolving-git-line-ending-issues-in-containers-resulting-in-many-modified-files).
  
  **Note**: Most guides now suggest you configure `user.email` at the same, but we will do that later.

- Your config settings are stored in `$env:USERPROFILE\.gitconfig`

- Want extra fancy prompt pimping? See [How to make a pretty prompt in Windows Terminal with Powerline, Nerd Fonts, Cascadia Code, WSL, and oh-my-posh](https://www.hanselman.com/blog/HowToMakeAPrettyPromptInWindowsTerminalWithPowerlineNerdFontsCascadiaCodeWSLAndOhmyposh.aspx)
 
## A high level overview of Git

- Git runs on Windows, MacOS, and Linux

- Git provides each developer with a local repository (repo):
  - Keeps a complete history of all the files in our project, the changes that occurred over time
  - The repo can manage branches with unique sets of isolated changes
  
- Git provides commands to add new changes, recover old versions and retrieve historical data

- Each Git repo can connect and share code with other repos managing the same project. The action of creating a local repo based on an existing project is referred to as cloning

- Because Git is distributed each repository clone has a (mostly) complete record of all changes

- But as repos are cloned amongst multiple users each repo may have their own unique history.

- Git maintains information about the other repos that it shares changes with in [remote](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes) tracking branches

- Git can handle large numbers of files (for example the GNU/Linux [kernel source code](https://git.kernel.org/pub/scm/linux/)). However if you have very large binary files then Git (or other general purpose VCS tools) may not be your best choice, but see [Git Large File Storage](https://git-lfs.github.com/).

- Technically Git repositories have a peer to peer relationship.
  In practice developers usually commit to a single upstream repository and
  multiple [workflows](https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows) can be build on top of this model.
  All changes can be shared with other repos as needed, usually to an "upstream" repo (by convention called `origin`)

- Code sharing sites like [GitLab](https://gitlab.com/), [GitHub](https://github.com/), and [BitBucket](https://bitbucket.org/) provide facilities for developers to co-operate across the Internet using upstream repositories

- Git repos either manage a working copy (e.g. a directory of project files on a developers workstation),
  or are bare repos (for instance located on GitHub) used to exchange changes between working copies and provide a "whole of project" view.
  - c.f. The [Subversion](https://subversion.apache.org/) VCS (and many others) is a centralised system with a single repo that all developers connect with to make changes

- Your local repo database is stored in `.git` directory, don't worry about it for now

See also [What is Git?](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F)

## Everyday Git commands you need on the PowerShell prompt, with examples

The Git command line interface consists of the executable  `git` followed by a command and the corresponding arguments and options.
There are many commands and a myriad of options so it can seem a little overwhelming all at once, we will focus on the basic workflow commands.

Note that the Git CLI follows UNIX/Linux conventions, not PowerShell.

There are many links to help you discover the details.

### Important commands

- [init](https://git-scm.com/docs/git-init) or [clone](https://git-scm.com/docs/git-clone)

  - `init` allows you to initialise a new git repo inside a project that is not already under version control e.g.

    `git init <project_dir>`

  - `clone`  clones the complete history of a remote project. You can now work on a running project. For example, let's clone the Git repo for these examples onto our workstation
  
    `git clone https://github.com/alecthegeek/git-from-powershell.git`

- [add](https://git-scm.com/docs/git-add) (plus `rm` and `mv`).

    Adding changes to a Git repo is a two stage process. All changes are staged in the index, before they’re committed into the repo.

    ![https://i2.sitepoint.com/graphics/1749-git-index-diag.thumb.png](https://i2.sitepoint.com/graphics/1749-git-index-diag.thumb.png)

    **Note: ALL changes, not just new files, need to be added to staged into the Index before they can be committed**

    `git add <file-name>` or

    `git add <directory-name>` to add the changes in a directory tree.

    Files can be renamed or moved with [`git mv ...`](https://git-scm.com/docs/git-mv), and deleted with [`git rm ...`](https://git-scm.com/docs/git-rm).

- [`commit`](https://www.git-scm.com/docs/git-commit)
  
  After a changes has been assembled (staged) in the index (using `git add`, `git mv`, or `git rm`) the change must be
  committed into the repo with the [`git commit`](https://git-scm.com/docs/git-commit).

  Note:

  1. **Before** committing your changes

     1. `pull` (or `fetch` and `merge`) any recent changes from your remote repositories (more on `pull` later)
     2. run any tests you have to make sure the change is correct

  2. During the commit operation provide a [useful commit message](https://chris.beams.io/posts/git-commit/)
  > a well-crafted Git commit message is the best way to communicate context about a change to
  > fellow developers (and indeed to [our] future selves).
  > A diff will tell you what changed, but only the commit message can properly tell you why
  > --<cite> [Chris Beams](https://chris.beams.io/)

- [`checkout`](https://www.git-scm.com/docs/git-checkout)
  
  The `git checkout` command allows you to move the current `HEAD` to another point in the repo history **or** create a new branch

  Note: `HEAD` is the pointer to the current state of the working copy in source control, but **without any changes you may have made in your working copy**. Git will often tell you about `HEAD`

  - To move you working copy to another point in history use `git checkout <history reference>` where the `history reference` is the name of an exiting branch,
    a tag, or some other reference to a previous commit the repo history.

  - To create a new branch use `git checkout -b new-branch-name`

- [`pull`](https://www.git-scm.com/docs/git-pull)

  The `pull` command downloads **and merges** changes from another [remote](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes) repository, usually the upstream "origin" repository hosted on GitHub, or a similar service.

  See also [`fetch`](https://www.git-scm.com/docs/git-fetch) which downloads the changes, but does **not** merge the remote changes.

- [`merge`](https://www.git-scm.com/docs/git-merge)

  Take the contents of two branches (the content must exist in your local repo) and combines them into single branch.
  Git will do it's best, but will need help to resolve conflicts if changes on lines overlap.
  More details [here](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging).

  See also [`branch`](https://www.git-scm.com/docs/git-branch)

Don't forget of course the [`git status`](https://www.git-scm.com/docs/git-status) and [`git log`](https://www.git-scm.com/docs/git-log)

## Storing and sharing your files on GitHub

The [GitHub](https://github.com) website provides [SaaS](https://en.wikipedia.org/wiki/Software_as_a_service) Git hosting. So you

1. Keep your local project repos on your workstation
2. Store the upstream [bare](https://git-scm.com/book/en/v2/Git-on-the-Server-Getting-Git-on-a-Server) project repos on GitHub (or some other similar SaaS service)

GitHub upstream repos can be managed from the PowerShell prompt

Install the GitHub CLI ([`gh`](https://cli.github.com/)) tool via Chocolaty

```
choco install gh
```

Now you can add your current project to GitHub

```
gh repo create --public
```

Push project code to GitHub

```
git push --set-upstream origin main
```

Now open the repository URL On GitHub.

## What to read or watch next

- [The Git Parable](https://tom.preston-werner.com/2009/05/19/the-git-parable.html). An introduction to the concepts behind Git

- A nice, rapid, intro to VCS, Git and GitHub for web projects — applies to any type of project

[![Git for web developers](https://img.youtube.com/vi/1u2qu-EmIRc/0.jpg)](https://youtu.be/1u2qu-EmIRc?t=463 "Rapid intro to VCS, Git and GitHub for web projects")

- A series of short videos introducing Git on PowerShell

[![Video Playlist](https://img.youtube.com/vi/WBg9mlpzEYU/0.jpg)](https://www.youtube.com/playlist?list=PLwNoYdA7KMWn0eLRG6lvp2Ir2npoCjRth "A series of short videos introducing Git on PowerShell")

- The Pro Git Book. Read online for free or buy a dead tree version

[![Pro Git Book](https://git-scm.com/images/progit2.png)](https://git-scm.com/book/)

- Simple intro to Git cheerpick

[![Git cherry pick tutorial. How to use git cherry-pick](https://img.youtube.com/vi/wIY824wWpu4/0.jpg)](https://youtu.be/wIY824wWpu4)
