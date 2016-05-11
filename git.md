# Git Cheat Sheet
## Reference
### rebase to last 2 head
```bash
$ git rebase --interactive HEAD~2
```
### reset base to commit
```bash
$ git reset --hard <commit>
```
### revert everything from the HEAD back to the commit hash
This is a safe and easy way to rollback to a previous state.
```bash
$ git revert --no-commit <commit>..HEAD
```
### revert changes to a submodule
```bash
$ git submodule foreach git reset --hard
# recursive flag to apply to all submodules
$ git submodule foreach --recursive git reset --hard
```
### checkout to commit id
Note: 'detached HEAD' state.
```bash
$ git checkout <commit>
```
### reset the staging area
```bash
$ git add hello.rb
$ git reset HEAD hello.rb
```
### amend the previous commit
```bash
$ git add hello.rb
$ git commit -m "Add an author comment"
$ git add hello.rb
$ git commit --amend -m "Add an author/email comment"
```
### git pull == git fetch && git merge @{u}
```bash
$ git fetch origin
$ git merge origin/master master
```
### shortlog
```bash
$ git shortlog
$ git shortlog -n|-s|-e
```
### playing with log
```bash
$ git log --pretty=oneline
$ git log --graph --decorate --oneline
$ git log --skip=2
$ git log --since=2015-07-01 --until=2015-07-02
$ git log --grep="Merge"
$ git log --all --pretty=format:'%h %cd %s (%an)' --since='7 days ago'
$ git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
# get log for last 2 commits
$ git log --pretty=format:"%h %s" HEAD~2..HEAD
```
### tagging (lightweight and annotated)
```bash
$ git tag edge_v1.1
$ git tag –a ann_v1.1 –m 'Annotated tag v1.1'
```
### archive repository
```bash
$ git archive master --format=zip --output=../backup-master-01-07-2015.zip
```
### view diffs between commits
```bash
$ git diff HEAD~2..HEAD~1
$ git diff master..john/pink-page
```
### stashing changes
```bash
$ git stash save "Grant Prestige Bonus"
```
### How to cherry-pick multiple commits
The simplest way to do this is with the onto option to rebase. Suppose that the branch which current finishes at a is called mybranch and this is the branch that you want to move c-f onto.
```bash
# checkout mybranch
$ git checkout mybranch

# reset it to f (currently includes a)
$ git reset --hard f

# rebase every commit after b and transplant it onto a
$ git rebase --onto a b

# from version 1.7.2
$ git cherry-pick 7f545188^..a7785c10
```
### Bisect
```bash
# Find commits that break you code fast
$ git bisect start
$ git bisect good <commit-id>
$ git bisect bad HEAD
# Test and flag the commit
$ git bisect good|bad
# Go back to the starting point
$ git bisect reset
# Get a summary report of last bisect run
$ git bisect log
```
### Remove untracked files and folders
```bash
# remove untracked files (matched all .txt files)
$ git clean -f -e*.txt
# To remove all untracked files from your working copy:
$ git clean -f
# To remove all untracked files and folders:
$ git clean -fd
# Tip: If you'd like to see first which files will be untracked before actually removing them, you can do a safe clean test by running:
$ git clean -n
```
## Aliases
### creating aliases
```bash
$ git config --local[--global] alias.st status
$ git st
```
### [My git aliases](https://gist.github.com/celavi/1ada8b067031b05733e45c624e503e56)
## Global templates
### gitignore
```bash
$ git config --global core.excludesfile ~/.gitignore_global
# Create the .gitignore_global file
touch .gitignore_global
# Go into edit mode so you can add the unwanted file listing
vim .gitignore_global
```
### [Template for commit messages](https://gist.github.com/celavi/4f73461976cc6fa90b1d9f5e051d2f8b)
```bash
$ git config --global commit.template ~/.git_commit_msg.txt
```
## Setup Line Ending Preference
### for Unix/Mac users
```bash
$ git config --global core.autocrlf input
$ git config --global core.safecrlf true
```
### for Windows users
```bash
$ git config --global core.autocrlf false
$ git config --global core.safecrlf true
```
## Snippets
### list offsets from HEAD with git log
```bash
o=0; git log --oneline | while read l; do printf "%+9s %s\n" "HEAD~${o}" "$l"; o=$(($o+1)); done | less
```
### delete all local branches except master
```bash
git branch | grep -v "master" | sed 's/^[ *]*//' | sed 's/^/git branch -D /' | bash
```
## [Git Housekeeping: clean-up outdated branches in local and remote repositories](https://gist.github.com/celavi/46645418f62b5f87967869d430e34b0a)
