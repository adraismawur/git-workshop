# Merging

This is a simple example to show you how two forms of merge strategies work, and how they affect the history of your branch.

## Fast-forwarding

The simplest manner of merging, is not really merging at all.
When you create a new branch and make changes, the commits that result from those changes exist on top of the preceding commit.
Because of this, GIT can simply apply those same changes to the branch which you're merging the changes into.

In order to see this in action, create a new branch based off of this branch using ```git branch merge-fast-forward```.

In this branch, create a new file under the examples/merge folder.


## Default merge strategy

By default, git

Once you are done with this example, type in ```git checkout main``` to return to the main branch.
