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


