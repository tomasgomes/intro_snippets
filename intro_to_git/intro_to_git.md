# Intro to version control using Git (and GitHub)  

When working on a project, it is important to keep track of its changes as we progress. Ideally, one should also store the most important files in an independent repository, in order to prevent losses due to storage issues, but also to improve sharing.  

One of the most widely used tools for this is Git. Git includes a variety of commands in order to control versioning of files in a directory, as well as annotating, merging, and sharing those files.  

Git works independently of [GitHub](https://github.com/) or [GitLab](https://about.gitlab.com/), but nowadays these are important parts of code sharing and maintenance.  

This tutorial will cover the basic commands of using Git through the terminal. It will also cover how to use Git within RStudio, and saving your projects to GitHub. The [Git Cheat Sheet from GitHub](https://education.github.com/git-cheat-sheet-education.pdf) may also be quite helpful.  
  
  

## Setup

A git repository can be created with `git init`. This will create a `.git` folder, where all project version changes will be tracked. In RStudio, git can be initialized by going to *Tools > Project Options > Git/SVN*. ![](./img/init_git_rstudio.png)  

Alternatively, we might wish to start a project from a pre-existing one (hosted on GitHub, for example). This can be achieved with `git clone [url]`. This URL can be obtained from GitHub directly, where we can also download a zip file of the project. ![](./img/git_url.png)  
  

## Staging and committing

Saved files are not automatically added to the project's history. To do this, we first need to use `git add [file]`. This will "stage" the file, i.e. prepare it to be included in the next snapshot. Several files can be added at each snapshot. Conversely, a file can be "unstaged" by using `git reset [file]`. In RStudio, one can use the Git menu to add files (as well as other commands here discussed). ![](./img/git_menu.png)  

This menu also shows you the "status" of the files. In the console, you can see this with `git status`, especially for the files that you have added but not yet commited to a snapshot. Committing a snapshot is done using `git commit -m "[descriptive message]"`. This will add all the "staged" files as a new snapshot. In RStudio, this is done by opening the Commit window in the Git menu. ![](./img/git_commit.png)  

In this menu, we can add our commit description, see the projects "History" (all snapshots), and see below what changed in each file. In the present example, we see the whole file in green, since all that text is new and nothing was deleted.  

It is also possible that we may want to disregard certain files when doing version control. A common example of this is when we store data it the same directory as our code, since these datasets tend to be too large to keep (several) versions in the project's history. To not consider a file or directory for version control, we can add their name to the `.gitignore` file (since it is a hidden file, i.e. starts with ".", in the terminal requires `ls -a` to be listed). Everything listed here will not be considered for staging. When creating a Git project in RStudio, some files will be added to this list by default, since they do not require version control.  

Lastly, accidents happen. We may eventually find ourselves in a situation where a file that has been previously commited should have never been (e.g. very large files, files with sensitive/confidential data, files that end up turning too large like some Jupyter NB). And even when we delete the file from our current directory and commit that deletion, the former versions of the file will remain in the project's history. To remove these files and their full history, we can use `git-filter-repo` (`conda install git-filter-repo`). This will add an option to git to do just that, e.g. `git filter-repo --force --invert-paths --path some_file.txt`.  
  

## Pushing to GitHub

Now that a new snapshot has been committed, it is helpful to have our project stored in a remote repository, where it can be easily accessible. This is where GitHub comes in.  

First, to 
`git remote add origin [url]`

100Mb file size limit (50Mb recommended)
