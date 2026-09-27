# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.

## CC-01 — "The search suggestions are behind everything"

**Reproduced:** I typed a dish name, waited for a sec and then the dropdown appeared , but its z-index was below the menu grid, which was hiding its visibility

**Cause:** I checked the style.css and i checked the z-idx of .suggest-box -> this thing has a z-idx of 100, BUT... the problem was that , the parent of this div , which is .search-wrap , had a z-idx of 1, and that was the main problem, coz it no matter what z-idx is given to the suggest-box it will be limited inside its parent, and the parent of menu bar has z-idx of 2, so it will appear on top of it .
Deeper Problem -> i thought that was it , but .cat-tabs having z-idx-40 became the real culprit.

**Fix:** I changed the z-idx of .search-wrap to 3, so that it has higher z-idx than its competitore which was #menu, but even changing the z-idx to 3 showed no change, that's when i realised that cat-tabs having z-idx of 40 is the main problem, even though it seems like cat-tabs is present on sideways of the menu bar, but its actually places bottom of the search bar which causes the z-idx conflict, i changed the z-idx to 41,and it worked flawlessly.

**Checked:** typed sa, and showingn samosa etc properly.

**Time:** about 40 minutes.




## Could not fix

For anything you investigated but did not solve. Say what you tried and
where you got to. This is worth marks — leaving it blank when you got
stuck is not.

### CC-0X — "<the complaint>"

**What I tried:**

**Where I got to:**

**What I would try next:**



## Extra credit

Anything not on the bug log: a problem you found yourself, a test you
wrote, or a fix you are unsure about. Same format, plus one line on how
you noticed it.
