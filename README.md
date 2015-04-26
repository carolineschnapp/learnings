As a prerequisite to this tutorial, you will need to:

* Create a new repo in your personal Github account. Call it 'learnings'.
* In that repo, add a README.md file with content in it.
* Clone the repo to your computer.

Do that, and when done come back here.

On Github you'll have something like this:

![Alt text](https://monosnap.com/file/X8tlyO5wmtjidy63drY61Ma7leUpyZ.png)

There are various stages where you might want to undo or tweak something you have done:

* STAGE ONE Before you commit anything.
* STAGE TWO After you've committed something to your `my-branch` branch, but before you publish that branch to your remote `origin/my-branch`.
* STAGE THREE After you have created a PR, but before you merge it.
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

# STAGE THREE After you have created a PR, but before you merge it.

**Blooper**

It will happen that you want to edit the commits in your PR before you merge your branch your branch to master. You may want to rename badly-named commits, or squash commits that make alterations into other commits. It's okay to have separate commits within a PR if each does its own 'main' thing, but not so much to have a long string of commits that reveal your trials and errors along the way to a solution.

**Solution**

While on your branch (the one you want to later merge to master), do a `git rebase --interactive HEAD~N` where N is the number of commits you want to revisit from now. If your PR contains 5 commits, and you want to revist them all, use:

`git rebase --interactive HEAD~5` # Will allow you to revist the last 5 commits on the branch you're on now.

Last command will open a file in your default Git editor. Want to use Sublime, or Textmate, and not command line? Before you run a rebase, tell Git which text editor you want to use: https://help.github.com/articles/associating-text-editors-with-git/

After running the last command, you'll have a file open that begins with something like this:

```md
pick 85c3423 changed my name again bit
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting
pick a5f4a0d renamed some file
pick b788002 added some file
```

To squash all commits into one, and rename the one remaining commit, edit the action names at the beginning of each line like so:

```md
reword 85c3423 a new better commit message that describes all that was done here and below
fixup f7f3f6d changed my name a bit
fixup 310154e updated README formatting
fixup a5f4a0d renamed some file
fixup b788002 added some file
```

It's pretty much self-exlanatory all that you can do to your commits from the comments in the file.

```shell
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
```

Then save and close the file.

Time to push your changes to your PR with:

`git push origin my-branch -f`

**Blooper**

After you created your PR, someone tells you your branch name is less than ideal — “Why didn't you prefix with the theme name?”. You've been alone contributing to your PR.

**Solution**

Start with this:

`git branch -m oldname newname` # This renames your branch on your computer.

Then time to delete the old branch on Github, and push your renamed branch to it.

`git push origin :oldname` # Delete the branch on Github.

`git push origin newname` # Publishes your local branch to Github.

Finally you'll need to create a PR **again**, because as far as Github is concerned that's a brand new branch. Don't forget to close the old PR.




