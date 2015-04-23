As a prerequisite to this tutorial, you will need to:

* Create a new repo in your personal Github account. Call it 'learnings'.
* In that repo, add a README.md file with random text in it — any text you like.
* Clone the repo to your computer.

That's it. We won't cover how the above is done, that's Github basics. Github will hold your hand through that process anyway.

Do that, and when done come back here.

On Github you'll have something like this:

![Alt text](https://monosnap.com/file/X8tlyO5wmtjidy63drY61Ma7leUpyZ.png)

There are various stages where you might want to undo or tweak something you have done:

* STAGE ONE Before you commit anything.
* STAGE TWO After you've committed something to your `my-branch` branch, but before you publish that branch to your remote `origin/my-branch`.
* STAGE THREE After you have published your branch to your remote `origin/my-branch`, but before you merge it to `origin/master`.
* STAGE FOUR After you have merged your branch to `origin/master`.

The actions you'll take to undo or tweak will vary depending on the stage you're at.

### STAGE ONE Before you commit anything

**Blooper**

Using whatever means, you edit the README.md file on your computer, but you are on the `master` branch or on a branch you did not mean to add to. You forgot to create a new branch and move to it before you started working. What now. You don't want to start over.

**Solution**

From the branch you worked on, do the following:

`git stash` # This will save all the work you have done but not committed, staged or not, to a Git “clipboard”.

`git status` # Optional, this should output "On branch whatever, nothing to commit, working directory clean".

`git checkout master` # Go to your local master branch. Terminal will spit "Already on 'master'" if you're already there.

`git pull` # Always do that before you create a new branch amiright.

`git checkout -b my-new-branch` # This will create a new branch with name my-new-branch and move to it.

`git stash apply` # This will copy the content of the Git “clipboard” to where you are now.

**Blooper**

You started working on the correct branch, done lots of shit, got lost, and want to start over with a clean slate. Nothing's been commited yet.

**Solution**

From the branch you worked on, do the following:

`git reset --hard HEAD` # This deletes untracked files and folders, except those ignored. This also reverts all edited files that are tracked so that they are identical to the ones in HEAD. HEAD is the last commit on the current branch. If you're on the branch my-new-branch, it's the last commit to my-new-branch. This is equivalent to selecting all and 'Discard changes to selected files' in the Github app.

If you only want to undo edits to a specific file:

`git checkout -- filename` # This resets one specific file to its previous state, i.e. HEAD state.

You can drag and drop a path from Finder to Terminal, so if you're a lazy or awkward typist you know what you have to do.

### STAGE TWO After you've committed something to your local `my-branch` branch, but before you publish that branch to Github.

**Blooper**

You just made a commit and you realize you made a mistake, either in what you changed, or how you named that commit. You want to change or nuke your last commit.

**Solution**

From the branch you commited to, do the following if you want to amend what you had commited:

`git reset --soft HEAD^` # This will undo the last commit but what was commited is back on stage so that you can edit all that, and commit again. Worth noting, the last commit is deleted from the Github history, so your honor is saved. After this, you will need to fix your stuff and commit your changs again.

What you had done was complete shit and does not need to be revisited. If you want to completely blow away the last commit and start fresh, use `--hard` instead of `--soft`:

`git reset --hard HEAD^` # This will undo the last commit and then do a git clean. There'll be no untracked files, and nothing on stage at the end of this.

The `^` means the last commit. If you want to blow away the last 2 commits, you'll use two of those:

`git reset --hard HEAD^^` # Nuke the last TWO commits.

If you want to blow up the last 3 commits:

`git reset --hard HEAD^^^` # Nuke the last THREE commits.

**Blooper**

You did not use the proper naming convention for your very last commit. You only want to edit the commit message.

**Solution**

`git commit --amend -m "My new commit message"` # Changes the commit message for your last commit.

**Blooper**

While we are on the subject of changing a commit _message_, how do you go about renaming the branch you are working on because the name does not respect the conventions or contains a typo.

**Solution**

Start with this:

`git branch -m oldname newname` # This renames your branch on your computer.

Then if that branch had been published on Github, continue on with these additional commands:

`git push origin :oldname` # Delete the branch on Github.

`git push origin newname` # Publishes your local branch to Github.

_Only rename a branch that you have been all alone contributing to, though._ 



