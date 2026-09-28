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
 

## CC-06: "I ordered more than they had"

**Reproduced:** I Clicked on grilled Sandwich, and the badge says 'only 2 left' but it surprisingly letted me add 10 of them in the cart(which is the max limit for any item). 

**Cause:**  There 2 Causes, in the problem state, the first part is : 'it let me order 5' meaning the bug is in fronted somewhere, and the second part 'the order went through fine' -> this one was tricky, coz the backed validated the request even when we ran out of stock. The overrall Cause was that the code was not checking 'dish.stock' properly at all the checks.

**Fix:** 1st -> Frontend fix : In the SetQty function added an additonal check for 'if(next > dish.stock)', so it also checks if the qty is greater than stock or not.
2nd -> Backend Fix : added 'if(qty > dish.stock)' .

**Checked:** Now there is a proper error when we try to add more items than present in the stock

**Time:** about 60 minutes.


## CC-07 - "Cancelling makes it worse"

**Reproduced:**  Added an item in the cart, ordered it, and then cancelled my order immediately, it should put the stock back, but it didn't.

**Cause:** The problem was simple, the stock was not getting refilled when we cancel our order, so i started searching with 'cancel', and my main motive is to find a function whose job is to 'put the stock back' and after searching for 10 minutes, i found a functions 'releaseStock' in validation.js, it logic seemed okay at first, but at line 123, it was 'subtracting' instead of adding the stock back -> this was the main bug.

**Fix:** In the releaseStock function, i changed the '-' to '+'

**Checked:** Added an item, ordered it, cancelled it. The stock came back.

**Time:** about 25 minutes.

 
 ## CC-08: "An old coupon still works"

**Reproduced:** I tried to use the 'FRESHERS24' coupon code in the checkout, and it Wroked, which is wrong, it should show an error.

**Cause:** searched coupon in the vs code search bar, and there was a lottttt of files, went through a buch of them, i was looking for something like 'new Date()', coz in order to check the expiry date of coupon, there must be this Date obj created and the main cause was -> the comparison which should check wether the current is expired or not in pricing.js was missing.

**Fix:** added new code in line 93 , in pricing.js, an if statement, if(new Date(coupon.expiresAt) < now), now, i used new Date(), coz this coupon.expirest at is an a different string format.

**Checked:** Now the coupon does not work and shows an error.

**Time:** about 50 minutes.
 

## CC-09 : "The menu shows more dishes than it should"

**Reproduced:** : Opened the menu, and there was written 'showing 37 dishes', so the error was pretty clear, too many dishes were being displayed.

**Cause:**  I started with 'menu' keyword, and searched a lot using vs code search feature, but didn't really found anything, then i thought of what is happening in the menu -> is is pagination, so i searched pagi.. , and then i came across this function 'paginate' in search.js, and there was an usused variable 'items' which was slicing properly, BUT, it was unused, in place of this var the code was passing 'list' which was the actual 30+ item list.

**Fix:** I replaced 'list' with 'items' in the paginate function, and it worked.

**Checked:** Now there is a proper pagination, and the menu shows the correct number of dishes.

**Time:** about 30 minutes.



## CC-10: "Sorting by price is backwards"

**Reproduced:**  Tried to sort the dishes by price low to high, BUT i got prices high -> low and the same happend in teh case of high -> low, it gave low -> high.

**Cause:** Since the logic revolves around Sorting and 'price-asc/price-dsc', i started searching them in the vs code search bar, and while searched i stumbled across search.js, in which at line 78 we have 'SORTERS', and when i looked closely at that, i saw the sorting logic has been reversed, the a.price-b.price was present in the price-asc, and vice-versa, which was the root of this problem.

**Fix:** Swapped a.price-b.price with b.price-a.price at lines 79 and 80 in search.js.

**Checked:** Now the sorting works properly.

**Time:** about 20 minutes.



## COULD NOT FIX
 
### CC-03 - "The menu is wider than my phone"

**What I tried:** I shifted to a phone using dev tools, and tried a to understand what this problem is, but i was not able to reproduced this problem, so i just decided to skip this problem.

**Where I got to:** Couldn't do much.

**What I would try next:** Will try on an actual phone.



**What I tried:**

**Where I got to:**

**What I would try next:**



## Extra credit

Anything not on the bug log: a problem you found yourself, a test you
wrote, or a fix you are unsure about. Same format, plus one line on how
you noticed it.
