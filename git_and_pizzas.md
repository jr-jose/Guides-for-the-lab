# Git/Github pizza recipes guide (very brief).

Using Git and Github is like making and delivering pizzas. Are you hungry?!??

(well, it's not like delivering pizzas but their recipes. Let's explain it ...)


### Table of contents.

+ [Basic ideas.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#basic-ideas-specific-language)
+ [What do you need?](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#what-do-you-need-in-your-gitgithub-kitchen)
    * [Minimal *Git* configuration.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#minimal-configuration-for-git-after-installation)
    * [Minimal *Github* configuration.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#minimal-configuration-to-connect-to-github)
+ [Basic workflow.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#basic-workflow-actions)
    * [Summary.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#summary-to-move-things-from-one-side-to-another)
    * [Tricks.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#tricks-to-keep-in-mind)
+ [Homework.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#homework-first-project-after-installing-and-configuring-git)
    * ['Doing'.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#first-steps-doing)
    * ['Undoing'.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#going-further-undoing)
    * ['Uploading'.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#available-everywhere-uploading)
    * ['Renewing'.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#usual-workflow-renewing)
    * ['Branching'.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/git_and_pizzas.md#contributing-to-someone-else-project-branching)


### Basic ideas (specific language).

* Your code/script is a recipe to make one of your usual/favorite pizzas. 
* Every recipe has a name (script name) and a creator (you?).


* Every recipe can be updated/improved; a ***commit*** is an update (remind it).
* Every *commit* has an identifier to document and trace the modifications.


* Recipes can fork in a main branch (***master***) and secondary ones.
* Git allows an easy way to trace these forks ... and to join branches later on.


* Your recipe versions are then easily traceable, and available from everywhere!
* Git is the versions control system most used worldwide, and it's for free.


Note I: a versions control system is a program allowing you to control 
developmental code/recipes changes, who changed it, when was do it, etc., 
making script versions always available from everywhere (and avoiding multiple
copies/versions of the same recipe in different computers). You will always know
which pizza version was cooked at every moment and from where it came! 

Note II: recipe forks allow multiple people working in the same project in parallel, merging all back to the *master* branch after the work is done.


### What do you need in your Git/Github kitchen? 

* A currently activated **Github** account: https://github.com/join .
* The **Git** software installed in your PC. .

Installation:

CentOs7 (in the terminal): `sudo yum install git`.

Windows: download (https://git-scm.com/downloads) and execute it.

Now, you are ready to use *git* commands! 

##### Minimal configuration for *Git* (after installation):
(use the terminal in Linux and the *Git Bash* software in Windows)

* Launch `git config --global user.name "Your name here"` command.
* Then, launch `git config --global user.email "your_email@here.com"`.


* Your configuration can be checked with `git config --global --list`.

##### Minimal configuration to connect to *Github*:

Instructions:

1. `cd ~`
2. `mkdir .ssh`
3. `cd .ssh`
4. `ssh-keygen -t rsa -C "your_email_address@crg.eu"`
5. `cat id_rsa.pub`

Copy the obtained code (it allow to connect to *github* from this specific PC).
 
Log in *github.com* with your previous credentials (https://github.com/login).

Go to the SSH keys section of your settings (https://github.com/settings/keys).

Add the copied code as *"New SSH key"* (please, use a descriptive title).

Launch `ssh -T git@github.com` in the terminal to check you can connect to it.

Done!


### Basic workflow (actions).

It's mandatory: every project should be a differentiated folder/directory.

1. Create the working/project directory with `init` (just once, not every time).
2. Save recipe changes with `add` (when you want, but try to do it often!).
3. Added changes are traced by using `commit` command (try to explain them!).


4. Share traceable recipes with `push` (recipe available everywhere via Github).
5. Download the available recipe version with `pull` (obtained from Github).


6. If you need *help* for a command, please use it (e.g.: `git help pull`).

##### Summary (to *move* things from one side to another):

There are three steps to arrive to the *summit*:

* From the *bench* to the disorganized *drawer* with recipes notes (to save it).
* From this *drawer* to your personal organized/traceable *recipes book*.
* From *your recipes book* to the *worldwide accessible (*wwa*) recipes book*.

Available commands:

| Kitchen bench | Recipes drawer  | Your recipes book  | *wwa* recipes book   |
| (workspace)   | (staging)       | (local repository) | (public repository)  |
| ------------- | --------------- | ------------------ | -------------------- |
| git init      |                 |                    |                      |
| git status    |                 |                    |                      |
| git add ..... | .......>        |                    |                      |
| git commit -a | ............... | ........>          |                      |
|               | git commit .... | ........>          |                      |
|       <...... | git reset *file*|                    |                      |
|       <...... | ............... | git reset *commit* |                      |
|       <...... | ............... | .................. | .......... git clone |
|       <...... | ............... | .................. | ........... git pull |
|               |                 | git push ......... | .........>           |
|               |                 |          <........ | .......... git fetch |
|       <.. git | diff ..>        |                    |                      |
|       <.. git | .... diff ..... | HEAD ...>          |                      |
|               |                 |       git log      |                      |
| ------------- | --------------- | ------------------ | -------------------- |

##### Tricks (to keep in mind):

* Use `git init` to work with current folder (you need to be inside the folder).
* Use `git init new_project_name` to create a new folder to work with.
* Use `git add file_name` to add a whole file to the project.
* Use `git add .` to add all files (and potential changes) recursively.
* Describe changes with a double `git commit -m "update name" -m "description"`.
* Save directly in local collections with `git commit -a` (no *git add* needed).
* Then, the `git log` command shows all traced changes in your local collection.


* The file *.gitignore* allows to specify always omitted files (e.g.: *\*.log*).
* A `git checkout -- file_name` eliminates bench changes (use very carefully!).


### Homework (first project after installing and configuring *Git*).

List of commands initializing your first project to start playing with *Git*.
Execute them and try to understand every single command and supplied argument.

* git commands are written in bold characters.
* use `cat file_name` command to see inside files (at any moment).
* use *git help git_command* to get specific help from a *git* command.
* and, finally, remember that ***git status*** is a very good friend too!

##### First steps ('doing'):

01. `cd pathway_to_your_desired_main_folder`
02. `mkdir hello`
03. `cd hello`
04. **`git init`**
05. **`git status`**
06. `echo "hello world!" > hello.txt`
07. `echo "This repository is a simple test." > readme.md`
08. `dir`
09. **`git status`**
10. **`git add .`**
11. **`git status`**
12. `echo "In fact, this is my first repository." >> readme.md`
13. **`git status`**
14. **`git add readme.md`**
15. **`git commit -m "Starting" -m "This is my first commit."`**
16. **`git status`**
17. `echo "I continue adding sentences to carry out tests." >> readme.md`
18. **`git commit -a -m "Continuation" -m "This is the second commit."`**

After step 04, the folder will always be under *Git* control (do it just once!).

After step 07, you had two files on your kitchen bench (see steps 08 and 09).

After step 10, these two files were waiting inside the recipes drawer (staging).

After step 12, you modified one of the files (see step 13).

After step 14, you saved the modification being present in the drawer/stage.

After step 15, everything was saved (and traceable!) in your local recipes book.

- this task should be done when a specific objective was reached to trace it -

After step 18, a new change was added to the book without being in the drawer.

- the *commit -a* option allows simultaneous stage and local book savings - 

##### Going further ('undoing'):

19. `echo "This line will be eliminated with 'checkout'." >> readme.md
20. **`git status`**
21. **`git checkout -- readme.md`**
22. **`git status`**
23. `echo "Modification to add to the stage/drawer but not kept." >> readme.md`
24. **`git add readme.md`**
25. **`git status`**
26. **`git reset HEAD readme.md`**
27. **`git status`**
28. **`git checkout -- readme.md`**
29. **`git status`**
30. **`git log`**
31. **`git log --oneline --graph --decorate --color`**

After step 21, changes in the bench (not added to stage/drawer) were discarded.

After step 26, changes were eliminated from the stage/drawer but bench kept.

After step 28, changes were also eliminated from the bench.

##### Available everywhere ('uploading'):

32. **`git remote add origin git@github.com:your_user/hello.git`**
33. **`git remote -v`**
34. **`git push -u origin master`**

After step 32, this local respository is linked with the new one on *Github*.

- you need to create the repository in the website too: https://github.com/new -

- please, create it but without adding any file (we will use the command line) -

After step 34, files/changes were send to the worldwide accessible repository.

- if you do not create the repository in the website, the *push* won't work -

- the *-u* option is necesary only the first time, but not for next updates -

- see your repository worlwide by visiting https://github.com/your_user/hello -

- you are also encouraged to makes changes on the website (e.g.: add a line) -

##### Usual workflow ('renewing'):

35. **`git pull origin master`**
36. `echo "New modifications that I'm doing locally." >> readme.md`
37. **`git status`**
38. **`git commit -a -m "Last line" -m "This commit modifies the last line."`**
39. **`git push -u origin master`**

After step 35, *Github* changes were also downloaded to your local repository.

- launching this command is very recommended before starting any work -

After step 38, changes were saved in the recipes drawer (stage) and local book.

After step 39, the modifications were also saved remotely (github repository). 

##### Contributing to someone else project ('branching'):

40. `cd ..`
41. **`git clone https://github.com/jr-jose/another_git_guide.git`**
42. `cd another_git_guide`
43. **`git branch another_git_guide_v_your_user`**
44. **`git checkout another_git_guide_v_your_user`**
45. `echo "Line that I'm adding to the project." >> git_and_pizzas.md`
46. **`git add git_and_pizzas.md`**
47. **`git status`**
48. **`git commit -m "modification XXX" -m "I just added a line for testing"`**
49. **`git push -u origin another_git_guide_v_your_user`**

After step 40, it's now you who should know and explore what to do ... :)


------------

Contact me at any moment if you have any problem or suggestion about all this.

Thanks!


