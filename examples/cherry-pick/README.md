# Cherry-picking

Cherry picking is a simple way of including specific commits into a branch from anywhere else.

This can be used in the following cases:

- You want to abandon a branch, but keep some of the work
- You want to rebuild a branch only with certain commits
- You want to merge someone elses work, bit by bit.

### Make an experimental branch

Create a new branch using ```git branch experiment```, and check it out using ```git checkout experiment```.
You can also combine these two operations by running ```git checkout -b experiment```

Create some files under examples/cherry-pick and commit them.
It would be good if you named them such that you can keep them apart.

E.g. create newfile1, and commit with the message "newfile1" by running ```git add examples/cherry-pick/newfile1``` ```git commit -m "create newfile1"```

Also try adding to one of the existing files in your commit.
Give the commit a message like ```add to newfile2``` so you can tell it apart from the rest.

### cherry-picking

Once you have a few commits (just a couple is fine!), checkout the main branch again by running ```git checkout main```

If you run ```git log``` now, you will not see any commits relating to the ```experiment``` branch.

You can view the changes you just made using either ```git log --all```, which shows you everything, or ```git log experiment```, which will show you the log of that branch directly.

Now cherry-pick the last commit you made where you created a file and see what happens.
Cherry-pick requires a hash.
Just like many other commands, this hash can be abbreviated to the first five characters if there are no duplicates with that same hash. e.g:

```git cherry-pick 49892```

Look at the contents of the examples/cherry-pick directory.
You should see that the file you created in the commit, on a different branch, is now present in the main branch.

Next try cherry-picking a commit where you edited a file, but did not create it.

You should see a message telling you that there is a conflict:

```
CONFLICT (modify/delete): examples/cherry-pick/newfile2 deleted in HEAD and modified in 45f1074 (newfile2-b).  Version 45f1074 (newfile2-b) of examples/cherry-pick/newfile2 left in tree.
error: could not apply 45f1074... newfile2-b
hint: After resolving the conflicts, mark them with
hint: "git add/rm <pathspec>", then run
hint: "git cherry-pick --continue".
hint: You can instead skip this commit with "git cherry-pick --skip".
hint: To abort and get back to the state before "git cherry-pick",
hint: run "git cherry-pick --abort".
```

This happens because you are not including the commit where you are creating the file, leading git to think that was once there has been deleted.

The message gives you a couple of hints on what to do.
If you want to keep the file, you first have to add it using ```git add```, e.g.

```git add examples/cherry-pick/newfile2```

Then you can continue the cherry-picking process using ```git cherry-pick continue```

The contents of your cherry-picked file should reflect the state of that file during the commit you cherry-picked.

