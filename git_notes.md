# git - useful notes and commands
---------------------
# Contents
- [Diff](#diff)
- [Customization](#customization)
- [Strange Error Messages](#strange-error-messages)
- [Initializing New Repos](#initializing-new-repositories)
- [Adding Changes to a Repo](#adding-changes-to-a-repository)
- [Commit](#commit)
- [Branches](#branches)
  - [Reachability](#reachability)
  - [Merging Branches](#merging-branches)
  - [Fast Forward Merge](#fast-forward-merge)
  - [Conflicts Merging](#conflicts-merging)
- [Remotes and GitHub](#remotes-and-github)
  - [Conflicting Changes LOCAL vs REMOTE](#conflicting-changes-local-vs-remote)
  - [Forking a Repo](#forking-a-repo)
  - [Pull Requests](#pull-requests)

## Diff
### Compare two files on Mac Terminal (not git)

```sh
diff -u filename1 filename2
# This shows line by line changes with + for new
# version of a line and - for old version
```
### Git diff

```sh
git log
# Displays a list of all commits, most recent first

git diff commit.ID.1 commit.ID.2
# git diff with no arguments returns comparison of
# changes between “staging area” and working directory

git diff --staged
# This compares staging area files with the last commit

git reset --hard
# This resets staging area and working directory changes
# back to the previous commit version.
# BE VERY CAREFUL! It is impossible to restore changes
# reset by this command
```

## Customization
```sh
git config --global color.ui auto
# Ensures we see color coded change log lines in git diff

git config --global core.editor "atom --wait"
# Configure Atom as default text editor

# Additional configuration settings
git config --global push.default upstream
git config --global merge.conflictstyle diff3

# Use .bash_profile file to set default git appearance
# and actions and store default settings
# Point .bash_profile toward
#       git-completion.bash
#       git-prompt.sh
```

## Strange error messages

```sh
"Should not be doing an octopus"
# Octopus is a strategy Git uses to combine many
# different versions of code together. This message
# can appear if you try to use this strategy in an
# inappropriate situation.

"You are in 'detached HEAD’ state"
# HEAD is what Git calls the commit you are currently
# on. You can “detach” the HEAD by switching to a
# previous commit, which we’ll see in the next video.
# Despite what it sounds like, it’s actually not a bad
# thing to detach the HEAD. Git just warns you so that
# you’ll realize you’re doing it.
```

## Initializing New Repositories

A Git repository contains a hidden system file, .git, that is typically not visible in the system viewer

This .git file contains metadata about the repository

```sh
git init
# Initializes a new empty .git repository in current directory
# At this point, no commits have yet been made
# You must create the first commit yourself

git --bare init
# Initializes a new 'bare' .git repository in current directory
# Bare repositories are not meant to be working directories
# Instead, when creating a remote 'origin' repository that will
#   will only be pushed to or pulled from, it should be a bare repo

git status
# Confirms that directory is actually a .git repository
# Shows which files have changed since last commit

git clone https://...git
# Clones a repository to your local drive

git checkout commitID
# Resets all files to the versions at the time a particular
# commit was made
```
## Adding Changes to a Repository

Git uses an intermediate __staging area__ to manage and provide control over changes

```sh
git add filename.ext
# The 'git add' command adds files to the staging
# area to await commit

git add -i
# Initiates an interactive 'git add' menu
# Useful for adding multiple files

git add -A
git add --all
# stages All, same as "git add .;git add -u" below
git add .
# stages new and modified, without deleted
git add -u
git add --update
# stages modified and deleted, without new

git reset HEAD -- [file-name]
# removes file (or directory) from the staging area
# modifications will be kept
# git status will then show file as modified but not staged
```
## Commit
__Q: How often should you Commit?__ <br>
__A: A good rule of thumb is one commit per logical change__
```sh
git commit
# Opens default text editor to enter commit message

git commit -m "Commit message text"
# Allows you to enter a commit message directly in the
# the command line without the use of a text editor
```
## Branches
Branching helps to keep your project organized, allowing the master branch to always work, while the developer context switches and compartmentalizes work (e.g. to work on a bug or develop a new feature)

```sh
git branch
# Lists available branches
# An asterisk next to a branch name specifies which
# branch that is checked out

git branch new-branch-name
# Creates new branch with specified name

git checkout new-branch-name
# Switches active branch to specified named branch

git checkout --b new-branch-name
# Shorthand that combines new branch and checkout
# into a single command

git log --graph --oneline branch-name1 branch-name2 …
# Provides visual representation of commits
# and sequence to multiple branches named
```
### Reachability

Reachability is the concept that commits are ‘reachable’ along a particular branch of commits.
  - Commits in separate branches are not reachable if the commits were made after starting the branch

  - DETACHED HEADs are commits made outside of established branches, and thus un-reachable in any branch git logs

### Merging Branches
```sh
# Go to branch you wish to merge another into
# example...
git checkout master

# Then merge in the other branch
git merge other-branch-name

# The original file from your checked-out branch
# is used to determine which differing lines should
# be included in the merged file.
#     i.e. what was in original and possibly removed in
#     one branch, and what was not there an possibly added
#     in a branch.

# Once we are done with a merge, we can delete the branch
# label for the branch merged into the other

git show
# For merged commits shows what commits were introduced
# for a particular commit vs. its parent commit (not the
# earlier commit made in a different branch)

git gc
# gc = garbage collection
# Deletes commit IDs for detached heads
```
### Fast Forward Merge
  - Any time commit 'b' is merged into 'a' and you can get to 'a' backwards from 'b'
  - The branch you are 'merging into' is an ancestor of a branch your 'merging from'

## Conflicts Merging
```sh
# When trying to merge two branches with conflicts,
# git will present...

<<<<<<<<<< HEAD
# This represents a section of my current file version

|||||||||||||||||||||| Merge common ancestors
# This represents a section that both versions modified

>>>>>>>>>>>> MASTER
# This represents a section in master version

# The diff between <<< and |||| are changes I made

# I should edit this code:
#   1. Delete git's special lines, to what the code should be
#   2. Save the resolved file
#   3. 'git add' the file to the staging area
#   4. 'git commit' the change to complete the merge
```
## Remotes and GitHub
```sh
# Setup the osxkeychain helper
git config --global credental.helper osxkeychain

# To exchange version commits I must set up my
# remote repository

git remote
# Lists existing remotes

git remote add name-of-remote git@SSH-URL
# Creates a new remote
#     It is customary to name first remote in
#     a git repository ‘origin’

# If you use git clone to create a local version
# of a GitHub repo, the remote is established
# automatically with the default ‘origin’ remote-name.
git clone git@SSH-URL

git remote -v
# 'v' stands for verbose, will provide more info
# including fetch and push URLs

git push remote-name branch-name
# Push local commits to GitHub repository

git push remote-name new-branch-name
# Pushes a different branch to GitHub

git pull remote-name branch-name
# Pulls all new GitHub commits to the local repository
#     This is the same as running 'git fetch origin' +
#     (plus) 'git merge local/master origin/master'
```
### Conflicting Changes LOCAL vs REMOTE
```sh
git fetch
# Fetches the remote changes and branches them
# locally as an origin/master branch

# Then 'git diff' can be used to compare local and
# remote diffs for potential conflicts

# Then git merge can be used to merge changes from
# remote into local master
#     i.e. 'git merge master origin/master'
```
### Forking a Repo
* Forking is a GitHub convention (GitHub repo to GitHub repo)
* Allows someone to make a copy of someone else’s repo to their own GitHub account


### Pull Requests
  1. Go to github, go to branch and click pull request for that branch, make determination as to what pull you are requesting (original non-forked repo or to one of your collaborators)
  2. You are then asking your collaborator to pull or merge the new branch into master
  3. “Merge Pull Request” option will only appear if there are no conflicts
  4. If I don’t want to merge as requested, I can make comments on github, either for the entire file, or for a specific line of code by clicking that line
  5. New branch is removed once merge is completed
