# Using the stash

Sometimes, you may want to drop what you are currently doing, and return to a clean working directory.
Or you may want to abandon a solution you are currently implementing and try something else, but don't want to get rid of everything you have done just yet.

Git's stash functionality is perfect for this.

We'll go through a few stash commands:

- Saving a stash using ```git stash push```
- Listing our stash using ```git stash list```
- Retrieving a stash using ```git stash apply```
- Retrieving a specific stash using ```git stash apply [name]```
- Deleting stash items using ```git stash drop``` and ```git stash pop```


### Saving a stash

Start by creating some new files under examples/stash.

If you run a simple ```stash push``` now, it will not save anything to the stash.
This is because by default, ```stash push``` does not include untracked files.

```
$ git stash
No local changes to save
```

Either ```git add``` and ```git commit``` your files, or use the -u option in ```stash push -u```

```
$ git stash -u
Saved working directory and index state WIP on main: d8da545 create stash folder
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

#### Naming your stash

You can give our stashes a useful name with the -m option.

First, create some new files again (your old files are in the stash).
Then try saving these files to the stash using ```git stash push -u -m "new files"```

```
$ git stash push -u -m "new files"
Saved working directory and index state On main: new files
```

### Listing the stash

In order to get a list of stashes you have saved, simply run ```git stash list```

```
$ git stash list
stash@{0}: On main: new files
stash@{1}: On main: wow
stash@{2}: WIP on main: d8da545 create stash folder
```

### Retrieving the stash
We can retrieve our latest stash (the one at the top) by using ```git stash apply```

```
Already up to date!
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        examples/stash/newfile2

nothing added to commit but untracked files present (use "git add" to track)
```

When you check our stash again using ```git stash list```, you should see that all the same stashes are there:

```
$ git stash list
stash@{0}: On main: new files
stash@{1}: On main: wow
stash@{2}: WIP on main: d8da545 create stash folder
```

### Retrieving a specific stash

You may want to retrieve an older stash.
This is done by using the identifier at the beginning of the stash list line, e.g. stash@{2}

```
$ git stash apply stash@{2}
Already up to date!
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        examples/stash/newfile1

nothing added to commit but untracked files present (use "git add" to track)
```

### Deleting stash items

At some point you may want to delete items from your stash. This is done using ```git stash drop```
Note that using ```git stash drop``` on its own will drop the most recent stash.

In order to delete specific stashes, use the identifiers the same way you did in _retrieving a specific stash_: ```git stash drop stash@{1}```

**Note: Be careful with these identifiers!**
**These are relative, so if you have three stash items and delete stash@{0} (the first), the most recent one will also have the identified stash@{0}**

If you are certain you want to retrieve your stash and no longer need it, you can retrieve and drop the stash simultaneously using ```git stash pop```. This also takes an identifier as an argument.

```
$ git stash list
stash@{0}: On main: wow
stash@{1}: WIP on main: d8da545 create stash folder
$ git stash pop
...
$ git stash list
stash@{0}: WIP on main: d8da545 create stash folder
```

