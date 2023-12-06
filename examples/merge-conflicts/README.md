# Causing trouble with merge conflicts

When working in a team, but also when working alone, it is likely you will run into a merge conflict sooner or later.

In this exercise, we will cause some merge conflicts so we can see what they look like.

### Same-line conflict

Start by creating a new file in examples/merge-conflict, and adding some text to it like ```hello, world!```.

Add and commit this file, then create a new branch using ```git branch edit-file```

Before switching to this branch, make a change to this file, e.g. by changing ```hello, world!``` to ```hallo, wereld!```.
Commit this change as well.

Now switch to the edit-file branch using ```git checkout edit-file```, and edit the file again.
Replace the ```hello, world!``` with a language of your choice.

```konnichiwa, chikyuu!```, ```hallo, welt!``` and ```hej, världen``` are the only other options I know.

Commit this edit and go back to the main branch using ```git checkout main```.


The situation is now as follows.
This may be familiar if you did the merge exercise as well:
```
      C edit-file
    /
A---B---D main
            ^ you are here
```

Try merging these branches using ```git merge edit-file```.
You should get an angry message like this:

```
Auto-merging test
CONFLICT (content): Merge conflict in examples/merge-conflict/test
Automatic merge failed; fix conflicts and then commit the result.
```

This message indicates that there is a conflict in the content of a file, that file being ```examples/merge-conflict/test```.

If you look at the contents of the file now, you will see the conflict itself:

```
<<<<<<< HEAD
hej, varlden
=======
hallo, wereld!
>>>>>>> edit-file
```

All this is, is git telling you that:
- There is a current change (from the branch you're on) under ```HEAD```
- This change has the content 'hej, varlden'
- There is an incoming change (incoming from the branch you're merging), above the name of the branch, which in this case is ```edit-file```
- That change has the content 'hallo, wereld!'

In order to resolve this conflict, you need to choose what you want to keep and what you want to remove.
In case you want to keep the current changes (from the main branch you're on):
- delete the ```<<<<<<< HEAD``` mark
- delete everything under ```=======``` up to and including ```>>>>>>> [branch-name]```

One thing to note in these cases is that you get to choose what to keep.
You can remove everything, keep one side, or keep both.
You can also pick and choose lines that you want to keep.
All you need to do is get rid of those lines starting with ```>```, ```=``` and ```<``` in the file that has a conflict.

Probably the simpler your solution is, the better.

Once you are done editing, save the file and use ```git add [file path]``` to stage the file.
Then you can run ```git commit``` to finalize the merge.


Sometimes GIT can create very strange merge conflicts within files, but they will always have some form like above.
Take a deep breath and try and understand what is coming in, and what changes it conflicts with.

If all else fails, you can always abort a merge by typing in ```git merge --abort```


### Conflicting files

Sometimes, you may have issues with a file being deleted in one branch, but edited in another.
This can happen for example when you have moved a chunk of code from one file to a new file, and then continued editing the old file in another branch.

We are going to emulate this situation, just to see what it looks like.

Make sure you don't have an active merging process from before.
If necessary, type ```git merge --abort``` to go back to some state before a merge.

Then follow these steps to create a merge conflict:

1. Create a new file with some content in it, e.g. two lines separated by newlines:

```
Hello,

world!
```

2. Commit this file
3. Create a new branch using ```git branch old-file-conflict```
4. Before checking out this branch, edit the file by adding some text in the middle, e.g.:

```
Hello,
big blue
world!
```

5. Commit this, then switch to the old-file-conflict branch using ```git checkout old-file-conflict```
6. Delete the file, and commit that change as well

Again we have created the same situation as twice before:


```
      C old-file-conflict
    /
A---B---D main
            ^ you are here
```

Except that in this case, you created a file in B, deleted it in C, and edited it in D.

Now you can try and merge these two branches.
If you want, you can try rebasing by using ```git rebase main```
If you want to stick to a normal merge, use ```git checkout main```, then ```git merge old-file-conflict```

Rebase result:
```
CONFLICT (modify/delete): examples/merge-conflicts/test deleted in d344bf0 (removed file) and modified in HEAD.  Version HEAD of examples/merge-conflicts/test left in tree.
error: could not apply d344bf0... removed file
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply d344bf0... removed file
```

Merge result:

```
CONFLICT (modify/delete): examples/merge-conflicts/test deleted in old-file-conflict and modified in HEAD.  Version HEAD of examples/merge-conflicts/test left in tree.
```

Note that there is a lot more output for the rebase, but the important bit is at the top.
The most important bit is this: ```Version HEAD of examples/merge-conflicts/test left in tree.```
This is telling you that the file that was edited, not deleted, is still there.
To resolve this conflict, you have two options:

- Keep the file, using ```git add <file>```
- Remove the file, using ```git rm <file>```

For now, delete the file and commit using ```git commit```

**Note: If you chose to rebase instead of merge, the rebasing process got interrupted by the conflict and you need to run ```rebase --continue```**

Remember that if you delete the file and merge, it is very easy to recover.
Rebasing can make it slightly more difficult, but it is still possible to recover a file that was deleted in this way.

### Recovering from a wrong conflict resolution.

Let's say you just rebased or merged the deletion, and realize that the change was actually super-important, and want to go back.

For both options, have a look at the log using ```git log```:

Merge:
```
commit ff7bac31789ecbd05878928d8f9c2710e2b27f25 (HEAD -> main)
Merge: 3fe26ed a700078
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   Wed Dec 6 08:58:43 2023 +0100

    Merge branch 'old-file-deleted'

commit a700078f31d09d1c9284dfb94708027fc0fd5685 (old-file-deleted)
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   Wed Dec 6 08:58:01 2023 +0100

    removed file

commit 3fe26ed1a6df8e667c98661749b918c8bb633fc7
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   Wed Dec 6 08:39:41 2023 +0100

    change file

commit fa0f42ece081e249829ac21ac8cd012564791eee
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   Wed Dec 6 08:37:58 2023 +0100

    add file
```

Rebase:

```
commit f2e572235b212edd63d60192ef95ea7b5d97f983 (HEAD)
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   Wed Dec 6 08:51:16 2023 +0100

    removed file

commit 3fe26ed1a6df8e667c98661749b918c8bb633fc7 (main)
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   Wed Dec 6 08:39:41 2023 +0100

    change file

commit fa0f42ece081e249829ac21ac8cd012564791eee
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   Wed Dec 6 08:37:58 2023 +0100

    add file
```

Reverting a merge commit is a little bit more involved.

In both cases you can revert the offending commit, using

Merge example: ```git revert a7000```
Rebase example: ```git revert f2e57```

Note that we are not reverting the merge in the merge example.
That comes with its own can of worms, and you can ask me about it if you are interested.

The following hard-to-read-but-very-interesting link also has some explanation:
https://github.com/git/git/blob/master/Documentation/howto/revert-a-faulty-merge.txt
