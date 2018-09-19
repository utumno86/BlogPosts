On Git
=======
A Crash Course By
[Michael E Nash](https://github.com/utumno86)

---

Who Am I?
=========

![Me and My Fiance With Our Cats in Christmas Hats](images/WithCats.jpeg)


---

Additional Cats
===============

![Very dirty black and white cat](images/HopperCat.jpg)

---------------

![Grey Cat](images/Shadowcat.jpg)

---------------
Class Resources
================

This presentation and all links in it will be available online
https://github.com/utumno86/BlogPosts/blob/master/GitPresentation.md

Presentation written with [Marp](https://yhatt.github.io/marp/)

-------------

What is a version control system?
=================================

From Wikipedia -

"... management of changes to documents, computer programs, large web sites, and other collections of information."

----------------------------


![git symbolt](images/git.jpeg)

------------
Linus Torvalds
==============
Invented git in 2005
![Linus Torvalds](images/linus.jpeg)

-------------

Installing Git
==============
- OsX 
	- install Homebrew [brew.sh](brew.sh)
	```
    brew install git
    ```
- Windows
  - install git for windows [https://gitforwindows.org/](https://gitforwindows.org/)

Git Website [https://git-scm.com/](https://git-scm.com/)

------

Configuration Overview
======================

- Personal info
```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```
- Editor
```
 git config --global core.editor emacs
 ```
- Aliases (optional)
```
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```
-------

Gitignore Global
================

```
git config --global core.excludesfile '~/.gitignore_global'
```
```
touch ~/.gitignore_global
```
Suggested Gitignore contents:
[https://github.com/github/gitignore](https://github.com/github/gitignore)

-----


Basic Git Commands
==================

```
git init
```
```
git add .
```
```
git commit -m "message"
```
----------

Git Branching
=============
![complicated diagramt](images/branching.png)

-------------

Git Branching Commands
======================

```
git checkout -b testing
```
```
git branch
```
```
git checkout master
```
```
git merge testing
```




  



