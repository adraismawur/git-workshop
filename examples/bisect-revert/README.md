# Bisect-revert

In this example, you will find a commit that introduced a mistake in this branch using ```git bisect```, and then revert it using ```git revert```.

The mistake in question is the dog that may be at the bottom of this readme file.
Scroll down to see what it looks like.

First start by typing ```git bisect start```.
You will now be able to tell git what is a good commit, and what is a bad commit.

Unless you know what the last good commit was, the easiest way to start is to just take the beginning of this branch.

You can find the hash of the first commit in this branch by using e.g. ```git log --pretty=oneline```.
This prints all the commits made in this branch, one per line, in a nice format that you can scroll up and down in using the arrow keys or pageup/pagedown.

If you do, you will find the hash ```9bb31ad854f70102826c8c804ec0d83abf0d21e7``` at the bottom.
You don't need to use the full hash, you can instead type in ```git bisect good 9bb31```

Since you are currently on a bad commit, you can set the current commit to bad using ```git bisect bad```.
If you do not add a hash to the end of a ```git bisect good``` or ```git bisect bad``` command, it will use the commit you are currently on.

Git bisect will now start the bisecting process.
You should see something like this:

```
Bisecting: 59 revisions left to test after this (roughly 6 steps)
[b147187186e27d31cd488d1bad4370888386c285] Morbi mollis felis sed tortor blandit finibus
```

Look at the bottom of this readme, is the dog there?
If so, type ```git bisect bad```.
If not, type ```git bisect good```.

Bisect will report how many commits are approximately left to check:
```
Bisecting: 3 revisions left to test after this (roughly 2 steps)
[78697b30ac0da558357b1cc84f8d506062f8d8bb] Donec rutrum urna gravida turpis vehicula, a rhoncus dui congue
```

When bisect has found your bad commit, it will show something like this:

```
[hash] is the first bad commit
commit [hash]
Author: Arjan Draisma <arjan.draisma@wur.nl>
Date:   [time]

    [message]

 examples/bisect-revert/README.md | 24 +++++++++++++++++++++++-
```

You can view the process using ```git bisect log```

```
git bisect start
# good: [9bb31ad854f70102826c8c804ec0d83abf0d21e7] first commit of bisect-revert
git bisect good 9bb31ad854f70102826c8c804ec0d83abf0d21e7
# bad: [bc7a776d0aad2348fe2a70025d789c9bc88a11a0] Donec condimentum quam commodo ultricies maximus
git bisect bad bc7a776d0aad2348fe2a70025d789c9bc88a11a0
# good: [b147187186e27d31cd488d1bad4370888386c285] Morbi mollis felis sed tortor blandit finibus
git bisect good b147187186e27d31cd488d1bad4370888386c285
# bad: [0f555727f58959f80c3039de58423937e360822d] Etiam rhoncus diam eget lectus sagittis interdum
git bisect bad 0f555727f58959f80c3039de58423937e360822d
# good: [0dbd833cdb3a832695286ebd979b2fe3b661f66d] Duis vel tortor tempus, condimentum felis a, volutpat arcu
git bisect good 0dbd833cdb3a832695286ebd979b2fe3b661f66d
# bad: [4a72ed805285f9ecea9a5333bb4b76eeab4f2875] Donec in ligula id nisl dictum dapibus convallis a arcu
git bisect bad 4a72ed805285f9ecea9a5333bb4b76eeab4f2875
# good: [78697b30ac0da558357b1cc84f8d506062f8d8bb] Donec rutrum urna gravida turpis vehicula, a rhoncus dui congue
git bisect good 78697b30ac0da558357b1cc84f8d506062f8d8bb
# bad: [a1aed464905d503825363c2e7b59a99aaef740e9] Lorem ipsum dolor sit amet, consectetur adipiscing elit
git bisect bad a1aed464905d503825363c2e7b59a99aaef740e9
# good: [6cc35039bb27313b3fffd0b7602c1d87b6e855dd] Curabitur tincidunt tellus eu ligula imperdiet, nec tristique libero sollicitudin
git bisect good 6cc35039bb27313b3fffd0b7602c1d87b6e855dd
# first bad commit: [hash] [message]
```

Take note of the bad commit hash!

Now all there is left to do is to get rid of this commit, which is done simply by typing:

```git revert [hash]```

This will generate a commit for you:

```
Revert "[message]"

This reverts commit [hash].

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch examples/bisect-revert
# Changes to be committed:
#       modified:   examples/bisect-revert/README.md
#
```

Accept this commit, and you're done!

Once you are done, check out the main branch by typing ```git checkout main``` in a command line.


Nothing weird here yet!
