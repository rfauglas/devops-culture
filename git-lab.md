
# Introduction
This is a Git practical lab to understand the basics of Git 
## Pre-requisite
[install git](https://git-scm.com/book/fr/v2/D%C3%A9marrage-rapide-Installation-de-Git) 
Unless you have a shell environment, make sure  you have a text editor...(notepad for ex...)

## Objective
Purpose is to:
- Understand the basic dataflow between:
- - local file systmem
- - staging area
- - git local repository
- Publish software change to repository 
- And resolve the conflicts.

## Time < 30mn

# Lab
## Create an empty repository
```shell script
mkdir test-git-repository
git init test-git-repository
cd test-git-repository
```
Inspect fresh repository
```shell script
git status
ls -lR
```
## If you use Intellij  (or some advanced IDE)
```shell script
echo ".idea/" > .gitignore
git add .gitignore
git commit -m "Do not import IDE property files in repo"
```
## Add empty file 
```shell script
touch file.txt
git status
git add file.txt
git status
git commit -m "new file empty file create"

```
Inspect log/ref
```shell script
git log
ls -lR .git
cat .git/refs/heads/master
```

## Rename file
```shell script
git mv file.txt README.md
git status
git commit -m "Respect documentation conventions"
git log
git show <commit_id>
```
## Edit file
```shell script

echo "This is my first static Web Site." >> README.md
git diff
git status
git add README.md 
git commit -m "MAJ documentation"
git log
git show <commit_id>
```

## Conflicts: merge
### First conflict: fix case change
```shell script
git checkout -b fix-case
echo "<html>
<body>
HELLO WORLD!
</body>
</html>"  > src/index.html
git add *
git commit -m "fix case"

```
and create another commit entry in history:
```shell script
echo "<html>
<body>
HELLO WONDERFUL WORLD!
</body>
</html>"  > src/index.html
git add *
git commit -m "wonderful fix"
```

### Another parallel conflict while cleaing

```shell script
git checkout master
git checkout -b clean-up
echo "<html>
<body>

Hello everybody!


</body>
</html>" > src/index.html
git status
git add *
git commit -m "Clean up"

```

### First merge on master branch 

```shell script
git checkout master
git branch -a
git log --graph --oneline --all
git merge clean-up
git log --graph --oneline --all
```
What happened?

### 2nd merge on master branch
```shell script
git merge fix-case
```
What happened?
```shell script
git status
```
Follow the hints to resolve conflicts. 
Open the conflicting files in an editor.
Git has detected and  "integrated" conflicting sections in the file... You have to clean up the file by yourself...
Note:
 - that your IDE might come with a handy conflict resolution tool...
 - You can force conflict resolution from the command line.
So what is a conflict?

```shell script
git add  *
git commit
```

## Conflicts: rebase
### create a branch for evolution 
and perform a wonderful evolution
```shell script
git checkout -b wonderful-evolution
echo "<html>
<body>
HELLO , WHAT WONDERFUL WORLD!
</body>
</html>" > src/index.html
git add *
git commit -m  "what intention"  
echo "<html>
<body>
HELLO , WHAT A WONDERFUL WORLD!
</body>
</html>" > src/index.html
git add *
git commit -m  "an intention"  
``` 
Come back  and change master:
```shell script

git checkout master
echo "<html>
<body>
HELLO WORLD, WONDERFUL WORLD!
</body>
</html>" > src/index.html
git add *
git commit -m  "World intention"
git log --graph  --all  
```
### Ready to rebase
Rebase sounds scary and magic: well you can always abort...



```shell script
git checkout wonderful-evolution
git rebase master
```
Ouch... Rebase failed! We have a conflict....
Ok so let's follow the advice and fix it. At any time:
```shell script
git status
```
Rebase is done?
```shell script
git log --graph  --all
```
ok let's continue rebasing on step further...
```shell script
git checkout master
git rebase  wonderful-evolution
git log --graph  --all
```

What happened?
How does it differentiate with a merge?
Would we have got a conflict when merging instead of rebasing?