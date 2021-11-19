---
title: "Getting Started With Git"
date: 2021-09-30T11:05:09+03:00
slug: getting-started-with-git
category: 
summary: Understanding Git and getting started with the basic commands on Git-Bash. This is a short introduction to Git, which you would have heard of on various forums if you are a beginner in coding and have been actively consuming lots of tutorials.
description:

cover:
  image: "/img/getting-started-with-git/git.gif"
  alt: "The Git Symbol"
  caption: "Get good at Git"
  relative: false
showtoc: true
draft: false
---

### What is Git?

<a href ="https://git-scm.com/" style="color:orange;" target = "_blank"><b>Git</b></a> is a ***distributed*** **version control system**. Version control systems (VCS) are software tools that help software teams manage changes to source code over time. There are many VCS available and Git is one of them, having been developed by [Linus Torvalds](https://en.wikipedia.org/wiki/Linus_Torvalds) in 2005. A VCS helps you do the following:

- Have a complete history of changes to your source code as a log.
- Branching and merging of the work on the source code into many streams, allowing various team members to work together parallel to each other at the same time. 
- Tracing each change made to the source code. (Developers use this to find bugs in their software)

This process is called source code management. I have used the [Atlassian guide](https://www.atlassian.com/git/tutorials/what-is-version-control) to learn Git, which I would highly recommend going through. 

### Installing Git

You can use [this guide](https://www.atlassian.com/git/tutorials/install-git) if you are using any other system than Windows to install Git on your computer. For Windows:

- Download the latest [Git for Windows installer](https://git-for-windows.github.io/).
- When you've successfully started the installer, you should see the Git Setup wizard screen. Follow the **Next** and **Finish** prompts to complete the installation. Choose default settings if you are a beginner. 
- 

### Setting up a repository 

Repositories are created in your project's *directorie* (folders like this --> 📁 in your PC. This is just a pictorial representation, you can have folder icons of many types). There are two ways through which you can get started with Git for your project. This is the place where the different versions of your updated projects are stored.  
In short, this is the history book 📓 of your project. 

1. You can start a Git repository in your project's directory. 
2. You can clone an existing project. 

### Git Commands

For this step you will need <a href ="https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.1" style="color:aqua;" target = "_blank"><b>PowerShell</b></a> and a working knowledge with [CLI](https://en.wikipedia.org/wiki/Command-line_interface). Working with `cmd.exe` is also fine, but I choose PowerShell, [here](https://www.howtogeek.com/163127/how-powershell-differs-from-the-windows-command-prompt/) are my reasons. You can also use [Git Bash](https://www.atlassian.com/git/tutorials/git-bash), a Windows app for emulating the Git CLI experience, to execute the commands. 


```
git config
```

This command is used to configure how you are presented on Git. There are three levels of git configurations, system, global and repository. If you are starting about, you will need these two configurations:

- `git config --global user.name <your name here, within "" if there is a space>` This will change the name and it will appear when in the edit history.
- `git config --global user.email <your email here>` This will add your email information to the commits. 

For all practicality, you would prefer using the `global` settings for most of your commits. But if you want to have a specific name and email associated with a directory, you can do that through the `git config --local user.name/email <info>` after opening PowerShell in that specific directory.

```
git init
```
This command will initiate a repository in the directory where you execute it. To do so use `cd <directory name>` (add the [path](https://docs.microsoft.com/en-us/dotnet/standard/io/file-path-formats) in between `< >`) to navigate to the directory of your project first.  Then execute this command. 

```
cd <directory name>
git init
```
Or you can use this command: `git init <project directory>` to initialise a Git repository in an existing project.  
`git init` is a one time operation in a project directory, consider it like a notebook that stores the entry of all the people in an office, you only need to have one register. 


```
git clone
```
As the name might have made it clear, this command is used to clone an existing project repository in a remote repository like <a href ="https://github.com/" style="color: BlueViolet;" target = "_blank"><b>GitHub</b></a> and obtain a local development clone. **Git** is the DVCS and **GitHub** is a service that hosts repositories for projects.  

```
git clone <repo url>
```
You can use Git <a href ="https://www.atlassian.com/git/tutorials/git-ssh" style="color: DarkTurquoise;" target = "_blank"><b>SSH</b></a> URL to do the same. You can use the link to know more about it if you are interested.  

```
git add <file name>
git commit -m "add a message here"
```
This command is used to add file version changes to your project's repository and then commit the change with a message attached to it. This way, you will have an idea why the commit was made. The `add` command basically gets your changes ready for being `committed` as changes to the remote repository. 

These are the basic commands that you need to know if you want to interact with the code of countless programmers out there in places such as <a href ="https://github.com/" style="color: BlueViolet;" target = "_blank"><b>Git</b></a>!  

Get git! 🚀



