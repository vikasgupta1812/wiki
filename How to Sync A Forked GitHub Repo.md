How to Sync A Forked GitHub Repo
====

Source - http://sta250.github.io/Stuff/sync_fork

### The Long Version

**Background**: Suppose you have forked the course repo, made changes, synced to GitHub etc. Everything is going well, until the course repo gets updated. Since your repo is a fork of the official course repo, changes made to the official repo will not appear in your fork with a simple `git pull`. Instead, to sync the changes from the official repo to your fork, you need to follow the instructions below. Note: these instructions are intended for use on Linux (e.g., Gauss) or Mac via the command line. For Windows users it is recommended to perform these steps on Gauss, push to GitHub and then sync to your laptop/desktop.

First, run `git remote -v` to check the status of your repo. You should see something like:
    
    
    $ git remote -v
    origin  https://github.com/yourusername/Stuff.git (fetch)
    origin  https://github.com/yourusername/Stuff.git (push)
    

Next up, add a remote that points to the upstream repo by typing:
    
    
    $ git remote add upstream https://github.com/STA250/Stuff.git
    

To check all went well, re-run `git remote -v` and you should see two new items:
    
    
    $ git remote -v
    origin  https://github.com/yourusername/Stuff.git (fetch)
    origin  https://github.com/yourusername/Stuff.git (push)
    upstream    https://github.com/STA250/Stuff.git (fetch)
    upstream    https://github.com/STA250/Stuff.git (push)
    

Now, time to fetch the new version of the official course repo:
    
    
    $ git fetch upstream
    remote: Counting objects: 6, done.
    remote: Compressing objects: 100% (3/3), done.
    remote: Total 4 (delta 1), reused 4 (delta 1)
    Unpacking objects: 100% (4/4), done.
    From https://github.com/STA250/Stuff
     * [new branch]      gh-pages   -> upstream/gh-pages
     * [new branch]      master     -> upstream/master
    

The changes are now on the local machine, but not yet merged. To check the branch status run:
    
    
    $ git branch -va
     * master                    a731cf7 Not much
       remotes/origin/HEAD       -> origin/master
       remotes/origin/gh-pages   8d936da Removed dead file
       remotes/origin/master     a731cf7 Not much
       remotes/upstream/gh-pages 8d936da Removed dead file
       remotes/upstream/master   522df7d Added fork sync link file
    

Next, check the branch you are currently on (should be master):
    
    
    $ git branch
     * master
    

If it is not the master branch then run:
    
    
    $ git checkout master
    

Finally, time to merge the changes to your repo:
    
    
    $ git merge upstream/master
     Updating a731cf7..522df7d
     Fast-forward
     Docs/Sync_Fork.md |    6 ++++++
     1 file changed, 6 insertions(+)
     create mode 100644 Docs/Sync_Fork.md
    

The latest changes to the official course repo (e.g., new homeworks) should appear in your local repo. If you run `git status` you should be ahead of the master branch:
    
    
    $ git status
    # On branch master
    # Your branch is ahead of 'origin/master' by 1 commit.
    #
    nothing to commit (working directory clean)
    

Lastly, to push the changes to GitHub, do the usual:
    
    
    $ git push
    

### The Short Version

In short, the first time you do this you need to run:
    
    
    git remote add upstream https://github.com/STA250/Stuff.git
    

Once you have done this once, just run the following:
    
    
    git fetch upstream
    git checkout master # you won't need this if you are already on the master branch
    git merge upstream/master
    

Note that this just updates the repo on Gauss (or whatever machine you are working on), not your GitHub repo. To push the changes to GitHub:
    
    
    git push
    

### The Fancy Version

To make life easier you can also use a Git alias. Open up your `~/.gitconfig` file using Vim or Nano and add the following lines to it:
    
    
    [alias]
        syncsetup = remote add upstream https://github.com/STA250/Stuff.git
        syncfork = "!git fetch upstream ; git checkout master ; git merge upstream/master"
    

You may already have other things defined in the file (e.g., I have [`user]` and [`credential]` defined), in which case just add the alias definitions below. Save the file and exit. Now, to set up the fork sync run:
    
    
    git syncsetup 
    

You only need to run this command once. If you get an error saying:
    
    
    fatal: remote upstream already exists.
    

then you already have setup the upstream branch. Next, to sync the fork use:
    
    
    git syncfork 
    

You should see a message along the lines of:
    
    
    $ git syncfork 
     remote: Counting objects: 7, done.
     remote: Compressing objects: 100% (3/3), done.
     remote: Total 4 (delta 1), reused 4 (delta 1)
     Unpacking objects: 100% (4/4), done.
     From https://github.com/STA250/Stuff
     522df7d..ccef041  master     -> upstream/master
     Already on 'master'
     Your branch is ahead of 'origin/master' by 1 commit.
     Updating 522df7d..ccef041
     Fast-forward
     Docs/Sync_Fork.md |    2 +-
     1 file changed, 1 insertion(+), 1 deletion(-)
    

As above, this syncs the local repo, but not GitHub. To update GitHub:
    
    
    git push
    

[1]: https://github.com/STA250/Stuff
[2]: https://github.com/STA250/Stuff/zipball/master
[3]: https://github.com/STA250/Stuff/tarball/master
[4]: http://sta250.github.io/Stuff
[5]: https://github.com/STA250
[6]: https://twitter.com/michigangraham
[7]: https://help.github.com/articles/syncing-a-fork

  

