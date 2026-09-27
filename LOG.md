# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.



## CC-01 — "The search suggestions are behind everything"

**Reproduced:** I typed a dish name, waited for a sec and then the dropdown appeared , but its z-index was below the menu grid, which was hiding its visibility.

**Cause:** I checked the style.css and i checked the z-idx of .suggest-box -> this thing has a z-idx of 100, BUT... the problem was that , the parent of this div , which is .search-wrap , had a z-idx of 1, and that was the main problem, coz it no matter what z-idx is given to the suggest-box it will be limited inside its parent, and the parent of menu bar has z-idx of 2, so it will appear on top of it .
Deeper Problem -> i thought that was it , but .cat-tabs having z-idx-40 became the real culprit.

**Fix:** I changed the z-idx of .search-wrap to 3, so that it has higher z-idx than its competitore which was #menu, but even changing the z-idx to 3 showed no change, that's when i realised that cat-tabs having z-idx of 40 is the main problem, even though it seems like cat-tabs is present on sideways of the menu bar, but its actually places bottom of the search bar which causes the z-idx conflict, i changed the z-idx to 41,and it worked flawlessly.

**Checked:** typed sa, and showingn samosa etc properly.

**Time:** about 40 minutes.

## CC-02 — "Can't read anything in dark mode"

**Reproduced:** I Toggled to Dark Mode and the first thing i saw was the greyed out text, which was highly unreadable.

**Cause:** Everything was fine in the 'html[data-theme="dark"]', the problem was in the line 294 of style.css, the color of dish-body was hardcoded to 2b2118.

**Fix:** I spent an hour trying to invent an 'if().else.' logic in css, until i stumbled across this 'var(--ink)', and then i was like, okay so wait, why the dish-body is having a hardcoded color? then i was like hmmm, it's the bug.

**Checked:** Dark mode now Works Propery with White color for the Name and Prices.

**Time:** about 50 minutes.


## CC-04 — "The buttons don't work on my tablet"

**Reproduced:** Switched to iPad in Dev Tools and found that the Add to Cart Button and the Star buttons were actually not working just on Tablets.

**Cause:** The cause was an 'Invisible Layer'. In the media query for tablets, there was CSS for the '::after' pseudo element of dish-card which was covering the buttons, making an invisible layer on top of the buttons making them unclickable. I found this by Inspect Mode; it showed "::after" only when I was in tablet view.

**Fix:** Googled and found 'pointer-events: none', which lets the clicks pass through the invisible wall, and it worked.

**Checked:** The buttons are working properly now.

**Time:** about 30 minutes.
 

## CC-05 — "The Category Filter Bar Scrolls Away on Mobile"

**Reproduced:** Switched to Phone by Dev Tools, and saw the Search Tab Scroll Away.

**Cause:** The cause was this 'overflow-hidden' in the @media for mobiles, this basically stop the 'position : sticky ' from working becuase of the nature of 'overflow' property.

**Fix:** I removed 'overflow: hidden' from the @media for mobiles and it worked.

**Checked:** Now the Scroll Bar Sticks on TOP.

**Time:** about 30 minutes.
 
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
