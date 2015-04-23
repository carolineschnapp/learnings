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

`git checkout master` # Go to your local master branch.

`git diff origin/master..master` # Optional. Check if other folks have done things to the remote master branch since last time you fetched changes.

`git pull` # Non-optional if the last command showed that stuff happened. Always a good idea before you create a new branch amiright.

`git checkout -b my-new-branch` # This will create a new branch with name my-new-branch and move to it.

`git stash apply` # This will copy the content of the Git “clipboard” to where you are now.




