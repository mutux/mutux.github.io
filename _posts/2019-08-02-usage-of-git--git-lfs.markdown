---
title: Usage Of Git & Git LFS
layout: post
---
I have been experiencing several issues related to github for the past several weeks. Fortunately, I've learned something as well, the following page is nothing but a personal note recording how github can be used in several circumstances.

## Create A New Repo from Local to Remote
  - Create a directory to contain the project, and go into it.
    -- ``mkdir mypro; cd mypro``

  - Initalize and configure the repository
    -- ``git init``
    -- ``git config --local user.email 'x@y.z'``
    -- ``git config --local user.name 'me'``

  - Write something, add and commit it to local repository
    -- ``touch test.py``
    -- ``git add .``
    -- ``git commit -m 'intital commit...'``

  - Create a repository 'myrepo' on Github, and write down the url
    -- 'git@github.com:me/myrepo'

  - Connect the local repo to the remote one
    -- git remote add origin git@github.com:username/new_repo

  - Push the local repository to the remote
    -- ``git push -u origin master``

## Create A New Repo from Remote to Local
  - Create a repository 'myrepo' on Github, and write down the url
    -- 'git@github.com:me/myrepo'
  - Clone the repository from remote to local
    -- ``git clone git@github.com:me/myrepo myrepo``
  - Configure the local repository if necessary
    -- ``cd myrepor``
    -- ``git config --local user.email 'x@y.z'``
    -- ``git config --local user.name 'me'``
  - Optional
    -- try pull and push to see everything works
      -- ``git pull``
      -- ``git push``

## Create a private repo
  - When you create the repository on github, remember to select 'private' instead of default 'public'
  - You may want to add your ssh key to your profile on github.
    -- ``pbcopy < ~/.ssh/id_rsa.pub``
    -- goto github.com->profile->setting->ssh and gpg keys->new ssh key-> paste


## Usage

### remove remote files with deleting local files
  - remove from local cache first
    -- ``rm -r --cache .`` or ``rm --cache 'filename'``
    -- add the names of the removed items into .gitignore
  - submit removal to remote repository
    -- ``git add .``
    -- ``git commit -m 'removing files'``
    -- ``git push``

### cancel committed files not submitted without affectiing local fiels
  - ``git reset``

### cancel last submitted files
  - ``git reset --soft HEAD~1``

### revert to previous version, and abandon current changes
  - ``git reset --hard HEAD~1`` or ``get reset --hard <version hash code>``

### dealing with conflicts while merging
  - ``git checkout --ours filename`` if to keep local file, or
  - ``git checkout --others filename`` to keep remotre file
  - ``git pull -s recursive -X ours``
    -- 'pull' does the 'download' and 'merge' together
    -- 'fetch' only download
    -- 'checkout' download a branch

## User large files on github

### Start using lfs
  - ``brew install git-lfs``
  - goto your local repo directory, and
    -- ``git lfs install``
  - select the files to be managed by Git LFS.
    -- ``git lfs track '*.tar.bz2'``
    -- ``git add .gitattributes``
  - start using it just like normal git

### untracking large files from lfs
  - remove a single file from
    -- ``git filter-branch --force --index-filter "git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA" --prune-empty --tag-name-filter cat -- --all``
    -- find and remove the associated rules in ``.gitattributes`` and save
    -- put the removed the file into '.gitignore'
    -- ``git lfs ls-files`` to view lfs files
    -- ``git rm --cached filename``
    -- ``git add .; git commit -m 'zzz'; git push``

## Tobe Continued.

## References
 - [Removing files from Git Large File Storage](https://help.github.com/en/enterprise/2.14/user/articles/removing-files-from-git-large-file-storage)
 - [Removing data from a repository](https://help.github.com/en/enterprise/2.14/user/articles/removing-sensitive-data-from-a-repository)
 - [Simple steps to uninstall Git LFS from your repository](https://github.com/git-lfs/git-lfs/issues/3026)
