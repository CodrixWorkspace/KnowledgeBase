# A Beginner's Guide to Git and GutHub

*Welcome to the world of collaborative programming! This guide is designed for absolute beginners to help you understand and use Git and GitHub effectively through the command line.*

---

## Understanding Git and GitHub

**Git** is a distributed version control system that allows multiple people to work on the same codebase simultaneously without overwriting each other's changes. It keeps track of every modification to the code in a special kind of database. If a mistake is made, developers can turn back the clock and compare earlier versions of the code to fix the mistake.

**GitHub** is a cloud-based hosting service that lets you manage Git repositories. It provides a web-based graphical interface and desktop as well as mobile integration. It also offers access control and collaboration features like bug tracking, feature requests, task management, and wikis for every project.

---

## Getting Started with Git and GitHub

### 1. Setting Up Git

Before you start, ensure that Git is installed on your computer. You can download it from the [official website](https://git-scm.com/downloads).

Configure your username and email:

        git config --global user.name "Your Name"
        git config --global user.email "youremail@example.com"

### 2. Creating a Repository on GitHub

- Log in to your GitHub account.
- Click the "+" icon in the top-right corner and select `New repository`.
- Enter a repository name and description.
- Choose between public or private visibility.
- Click `Create repository`.

### 3. Cloning a Repository

To make a local copy of the repository:

        git clone https://github.com/your-username/your-repository.git

Replace your-username and your-repository with your GitHub username and repository name.

### 4. Creating a Branch

Branches allow you to work on different parts of a project without affecting the main codebase.

        git checkout -b feature-branch

This command creates a new branch called feature-branch and switches to it.

### 5. Making Changes and Committing

After editing files, you can check the status of your changes:

        git status

To add your changes to the staging area:

        git add .

Commit your changes with a message:

        git commit -m "Add new feature"


### 6. Pushing Changes to GitHub

Push your branch to the remote repository:

        git push origin feature-branch

### 7. Creating a Pull Request

- Go to your repository on GitHub.
- Click on "Compare & pull request".
- Add a title and description for your pull request.
- Click "Create pull request".

### 8. Merging Pull Requests

If you have write access:

- Review the pull request.
- Click "Merge pull request".
- Confirm the merge by clicking "Confirm merge".

## Common Git Commands

- Deleting a Branch Locally and Remotely

        # Delete local branch
        git branch -d feature-branch

        # Delete remote branch
        git push origin --delete feature-branch

- Rebasing

        git checkout feature-branch
        git rebase main

- Merging

        git checkout main
        git merge feature-branch

## Why Use the Command Line with Git?

Using the command line for Git operations offers several advantages:

- Efficiency: Faster execution of commands without the need for graphical interfaces.
- Scripting: Ability to automate tasks using scripts.
- Advanced Features: Access to the full suite of Git functionalities.
- Learning: Better understanding of how Git works under the hood.
