# Merging

This is a simple example to show you how three forms of merge strategies work, and how they affect the history of your branch.

## Fast-forwarding

The simplest manner of merging, is not really merging at all.
When you create a new branch and make changes, the commits that result from those changes exist on top of the preceding commit.
Because of this, GIT can simply apply those same changes to the branch which you're merging the changes into.

In order to see this in action, create a new branch based off of this branch using ```git branch merge-fast-forward```.

In this branch, create a new file under the examples/merge folder.


Commit the file using ```git commit -m "added new file"```

You should see something like this:

```
[fast-forward 40bfa1e] added new file
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 examples/merge/newfile
```

Now checkout the main branch again using ```git checkout main```

To recap: we created a new branch, checked out the new branch, made a commit with a new file, and checked out the old main branch again.

The situtaion is now roughly like this:

```
A---B main
     \---C merge-fast-forward
```

With the ```git merge``` command, we are merging _into_ the current branch _from_ a branch we specify. So to perform the merge, run ```git merge fast-forward```.

You should see something like this:

```
Updating fe4596c..835e9b1
Fast-forward
 examples/merge/newfile | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 examples/merge/newfile
```

The ```Fast-forward``` here tells you that we did indeed fast-forward our branch with the new changes.
The new situation will look something like this:


```
A---B---C main & merge-fast-forward
```

Because all that happened is that commit C was fast-forwarded on top of the changes present in main.

## Default merge strategy (3-way)

A more complicated merge is performed when the histories of two branches _diverge_.
This happens when you have two branches with a common commit ancestor, but with different commits after that ancestor.

Let's re-create that situation.
Run ```git checkout -b diverging-branch```.

This command is a shorthand way of executing the commands ```git branch diverging-branch``` and ```git checkout diverging-branch```.

Create a new file in the examples/merge folder again, stage it and commit it like in the fast-forward section.

Check-out the ```main``` branch again using ```git checkout main```.

Create a new file here as well.
Make sure to name it something other than what you named the file in the ```diverging-branch```.

Stage and commit the new file.
The situtaion is now roughly like this:

```
      C diverging-branch
    /
A---B---D main
            ^ you are here
```

What this means is that diverging-branch and merge have a diverging history.
You can see this illustrated in a primive graph view by using the command ```git log --graph --all```.

You should see something like this:

```
* commit 17e8b32284d375bb6c5e218586893d5428fcb719 (HEAD -> main)
| Author: Arjan Draisma <arjan.draisma@wur.nl>
| Date:   Mon Dec 4 22:46:33 2023 +0100
|
|     new file
|
| * commit 1158e70ced8e66abe97742b772fe83a239a713f2 (diverging-branch)
|/  Author: Arjan Draisma <arjan.draisma@wur.nl>
|   Date:   Mon Dec 4 22:43:17 2023 +0100
|
|       new file
|
* commit 835e9b10511c4ad930358b62b75164c880f3a863 (main)
| Author: Arjan Draisma <arjan.draisma@wur.nl>
| Date:   Mon Dec 4 22:36:12 2023 +0100
|
|     update readme
|
```

Every asterisk indiciates a commit.
Notice that the diverging-branch does not have an asterisk where the branch diverges, but rather after the last common ancestor commit (835e9)

Let's merge these two branches together by merging diverging-branch into master.

```git merge diverging-branch```

You should be prompted with a commit message that was generated for you, e.g.

```Merge branch 'diverging-branch' into main```

Save this commit.
You can see the results of this merge by running ```git log --graph --all``` again.

It will look something like this, with a fork and a merging fork:

```
*\  commit 7c3bc6c9ed9ced254e9ad54c38295b112dd51f22 (main)
| | Author: Arjan Draisma <arjan.draisma@wur.nl>
| | Date:   Mon Dec 4 22:58:33 2023 +0100
| |
| |     Merge branch 'diverging-branch' into main
| |
* | commit 17e8b32284d375bb6c5e218586893d5428fcb719
| | Author: Arjan Draisma <arjan.draisma@wur.nl>
| | Date:   Mon Dec 4 22:46:33 2023 +0100
| |
| |    new file
| |
| * commit 1158e70ced8e66abe97742b772fe83a239a713f2 (diverging-branch)
|/  Author: Arjan Draisma <arjan.draisma@wur.nl>
|   Date:   Mon Dec 4 22:43:17 2023 +0100
|
|       new file
|
* commit 835e9b10511c4ad930358b62b75164c880f3a863 (origin/main)
| Author: Arjan Draisma <arjan.draisma@wur.nl>
| Date:   Mon Dec 4 22:36:12 2023 +0100
|
|     update readme
|
```

## Rebasing

Rebasing is a little bit more complicated.
Rebasing is a very powerful tool that lets you change the history of your branches.

This can result in nice, clean, linear commit histories, but can be confusing to work with.

In this example, we will keep it simple.
Repeat these steps from the previous merge example:

Run ```git checkout -b rebase-branch```.

Create a new file in the examples/merge folder again, stage it and commit it.

Check-out the ```main``` branch again using ```git checkout main```.

Create a new file here as well.
Make sure to name it something other than what you named the file in the ```rebase-branch```.

Stage and commit the new file.
The situtaion is now roughly like this:

```
      C rebase-branch
    /
A---B---D main
            ^ you are here
```

Rebase works slightly different in that by default it will play the changes of the current branch onto whatever branch you specify using ```git rebase [branch]```.

This means we have to check out the branch we want to rebase onto main using ```git checkout rebase-branch```.

Now simply use ```git rebase main```.
What this does is the following:

1. rewind the ```rebase-branch``` to the common ancestor commit (B)
2. replay the changes of ```main``` (D) onto the rewinded ```rebase-branch```
3. replay the changes of ```rebase-branch``` (C) again

the situation then will be as follows:

```
A---B---D main
         \---C rebase-branch
```

Does this look familiar? This is almost the same situation as the fast-forwarding case.
This means that we can also simply fast-forward the main branch!

Checkout the main branch using ```git checkout main``` and do a fast-forward merge using ```git merge rebase-branch```

Run ```git log --graph``` again to see the effects of this process. There are no forks in the history!

