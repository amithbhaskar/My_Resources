# Git Commands

Last Updated on : 12 May 2019

## Most Used Commands

### Initialze the Folder
`$ git init`

### Setting your username in Git
`$ git config --global user.name "Amith Bhaskar"`
> Amith Bhaskar@DESKTOP-SO75LRP MINGW64 /c/Amith/Github/Cheatsheets (master)

### Setting your email in Git
`$ git config --global user.email "amithbhaskar.312@gmail.com"`

### Check Username
` $ git config --global user.name `
> Amith Bhaskar

[Setting up a username in Git](https://help.github.com/en/articles/setting-your-username-in-git)

### Set the Github Project as Remote Origin
`$ git remote add origin https://github.com/amithbhaskar/My_Resources.git`

### Pull files from an existing project
` $ git pull https://github.com/amithbhaskar/My_Resources `
> From https://github.com/amithbhaskar/My_Resources 
> branch HEAD -> FETCH_HEAD

- Git pull command pulls down from a remote and instantly merges
- Git fetch is similar to pull but doesn't merge. i.e. it fetches remote updates (refs and objects) but your local stays the same (i.e. origin/master gets updated but master stays the same).

### Add Particular File
`$ git add README.md`

### Add Messages regarding the uploads and commit
` $ git commit -m "first commit amith" `
> [master (root-commit) 48b0796] first commit amith
> 2 files changed, 2 insertions(+)
> create mode 100644 README.md
> create mode 100644 asd.txt


### Upload the Files to the Github Project
` $ git push -u origin master `
> Counting objects: 4, done.
> Delta compression using up to 4 threads.
> Compressing objects: 100% (2/2), done.
> Writing objects: 100% (4/4), 267 bytes | 0 bytes/s, done.
> Total 4 (delta 0), reused 0 (delta 0)
> To https://github.com/amithbhaskar/My_Resources.git
> [new branch] master -> master
> Branch master set up to track remote branch master from origin.


### To add another file to the Project
` $ git add filename.ext `

### After adding the file, attach a message and commit
` $ git commit -m "msg amith" `

### Upload the committed files
` $ git push -u origin master `

## Formatting a Github md Page
[Basic writing and formatting syntax](https://help.github.com/en/articles/basic-writing-and-formatting-syntax#quoting-code)
