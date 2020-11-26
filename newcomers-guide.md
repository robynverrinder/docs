# Newcomers guide

A guide to using git and GitHub, along with some sample workflows. It's purposefully simple:
- the target audience is someone who programmed in undergrad using eg. Python, but doesn't know much about git
- it's fine to just link to existing documentation/blog posts/videos, unless they do a bad job of explaining things
- we should be realistic about the fact that researchers often work alone, or with a small number of collaborators. Complicated "best practices" that you read online are often written by people working at a large companies with many coworkers, so let's not get bogged down in admin unnecessarily

----

## Short intro to Git

[Git](https://git-scm.com/) is a software version control system commonly used to manage code. [Microsoft's guide](https://docs.microsoft.com/en-us/learn/modules/intro-to-git/1-what-is-vc) has this to say on why it's is useful:

    With a version-control system, you can:
    - See all the changes made to your project, when the changes were made, and who made them.
    - Include a message with every change explaining the reasoning behind it.
    - Retrieve past versions of the entire project or individual files.
    - Create branches, where changes can be made experimentally. This feature allows several
      different sets of changes (for example, features or bug fixes) to be worked on at the
      same time, possibly by different people, without impacting the master branch. Later,
      you can merge the changes you want to keep back into master.
    - Attach a tag to a version - for example, to mark a new release.

If you're not planning on using Git (perhaps because you're advising/talking to students who use Git) then just know these definitions (taken from [here](https://neros.dev/blog/git-crash-course-part-1/)) before skipping to the next section: [Short intro to GitHub](#short-intro-to-github)

- **Repository (aka "repo"):** Git works by designating a particular folder as a "repository", which means that it will track changes to any files in that folder or its children
- **Commit:** a commit is simply a bundle of changes, packaged up together and given a message to describe what those changes represent. This is the basic unit of organisation in Git's model of changes.
- **Clone:** the act of downloading a Git repository onto your computer
- **Fork:** the act of making a new copy of a Git repository

There are many guides to teach you the basics. For example:

- The official [git documentation](https://git-scm.com/doc)
- A to-the-point [20 minute crash course](https://neros.dev/blog/git-crash-course-part-1/)
- Microsoft's hour-long [Introduction to Git](https://docs.microsoft.com/en-us/learn/modules/intro-to-git/)

Some guides are long, but it's well worth it to just spend an hour going through one of them, since you'll likely use it a lot in your career. Even if you think you know the basics, please read the rest of this guide. From this point on we'll assume you have Git installed.


### Common files

Most projects have (or should have) the following files:


#### The `README.md` file

The first documentation any user will read about the project. Usually contains short and long descriptions, how to get set up (where/how to install software, IDE configuration, etc), usage examples (code samples and/or pictures and videos of the application). An example file is [here](https://github.com/alknemeyer/optoforce/blob/main/README.md)

There isn't a strict way to do things, so just ask yourself: if I didn't know anything about the codebase and only a little about the project, what documentation would I need to get fully set up? You can test this out by giving a friend access to the project and seeing whether they can quickly and easily get set up without asking you further questions

This file is super important! Please don't forget it! If you work on a project, don't document it properly, and then leave the university, your project will likely never be touched again


#### The `.gitignore` file

Not everything in a Git repository should be tracked. A rule of thumb is: "don't track files and folders which are automatically generated". Another rule of thumb is: if you modify one source file and build/run the project, other files that change as part of that process should not be committed

For example, you generally shouldn't upload:
- generated binary files (for example, `.elf` files)
- debug folders filled with random generated files
- `__pycache__/` and `.ipynb_checkpoints/` folders (which pop up when using Python and Jupyter Notebooks)
- log files (although important data should be added if the file size isn't too large)

Constantly commiting changes to these files introduces noise, making meaningful changes hard to track


#### The `LICENSE` file

If you want others to be able to use the code in your project, you should add a file named `LICENSE` with the corresponding contents inside. An example is [here](https://github.com/alknemeyer/optoforce/blob/main/LICENSE). [https://choosealicense.com/](https://choosealicense.com/) has a quick guide to help you choose one


#### Submodules

From [the documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules): "It often happens that while working on one project, you need to use another project from within it. Perhaps it’s a library that a third party developed or that you’re developing separately and using in multiple parent projects. A common issue arises in these scenarios: you want to be able to treat the two projects as separate yet still be able to use one from within the other."

Or in other words: sometimes you'll need to have a copy of another Git repository downloaded to your computer (for example, a project in the lab requires [BasicLinearAlgebra](https://github.com/tomstewart89/BasicLinearAlgebra) as a dependency). While you can just download or `git clone` it, the "right" way to do things is via [Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), which basically mean you can keep treating the your project and the dependency separately, and easily keep track of changes in the dependency

A short guide on using Submodules is [here](https://github.com/alknemeyer/typesieve#installation)

-----

## Short intro to GitHub
## TODO!

The official "Getting started with GitHub" guide is [here](https://docs.github.com/en/free-pro-team@latest/github/getting-started-with-github)


### Adding new users
- Permission levels (owner, member)
- People who aren't part of organization
- By default, people aren't publicly members


### Creating a repository
https://github.com/new


#### Public vs private
Repositories are private by default, which means that only members of the organization can see them


### Working with existing repositories

#### Viewing changes

#### Issues and Pull Requests

------

## Sample work flows

### Visual Studio Code

1. Open the Git panel (either by clicking the icon on the left, or by pressing `ctrl+shift+G`)

1. Assuming there's currently no repository, instructions will pop up telling you how to create one. Super simple!

1. Next, create or modify a file and then save it. The file name will pop up under "Changes" in the Git tab (if it doesn't, click the "Refresh" icon). Click on the file to view a the changes (green = added, red = removed, blue = modified)

1. If you want to commit all the changes, click the "+" icon to Stage the entire file. Otherwise, if you only want to commit a few lines, open the file, select the changes lines using your mouse or keyboard, then right click and press "Stage Selected Changes". Either way, a snapshot of the changes files will appear under "Staged Changes" in the Git panel.

1. Next, write a message in the Message box. The first line should be less than 50 characters (ie, just a brief description summarizing the change) but you can leave a blank space and before adding a longer, more descriptive message. Press `ctrl+Enter` to commit the change. You now have a new commit on your computer.

1. To synchronize changes to GitHub, press the double loopy arrow in the bottom left of your screen. Assuming there aren't any conflicts, VS Code will push/pull changes for you. Open the repository in your web browser if you want to confirm that everything worked

1. If you want to create branches (which is a good idea, but arguably unnecessary for smaller single-person projects when you're still new to Git) then click "master" or "main" (or whatever the branch you're working with is called) in the bottom left of the screen. It should be some text just right of a Git icon. Type the name of your new branch, or select and existing one to change to


### Command line interface

<!-- Honestly, I use the interface in vs code so I'm not really the best
person to write about it. Please improve this by adding your actual workflow -->

1. Launch a terminal/command window and navigate to your project's directory

1. Assuming there's currently no repository, just type
   ```bash
   git init
   ```
   to create one

1. Next, create or modify a file and then save it. Type `git status` to see what happened - it will tell you about new, modified, deleted and untracked files. If the file (which we'll call "myfile.txt") isn't tracked by Git, type `git add myfile.txt`. Finally, you can view the changes by typing `git diff myfile.txt`. If the file was newly created, no output will be shown

1. To stage the changes in the file, type
   ```bash
   git add myfile.txt
   ```

1. To commit the change, type
   ```bash
   git commit
   ```
   which will open a file editor for you to write a message. Otherwise, type
   ```bash
   git commit -m "enter your message here"
   ```
   to do it without opening a file editor

1. To synchronize changes to GitHub, type
   ```bash
   git push
   ```

1. If you want to create branches (which is a good idea, but arguably unnecessary for smaller single-person projects when you're still new to Git) type
   ```bash
   git branch name-of-new-branch    # create the branch
   git checkout name-of-new-branch  # switch to it

   # or in one command:
   git checkout -b name-of-new-branch
   ```
   existing branches can be viewed by just typing
   ```bash
   git branch
   ```
   but I'll leave a more detailed tutorial on that to the links way above
