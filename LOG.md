# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.

Delete the example before you submit.



## Example — delete this

### CC-99 — "The cart total is wrong"

**Reproduced:** Added 2 dosas at Rs. 60 each. The cart showed
Rs. 119.99999 instead of Rs. 130. Happened every time, on any dish with
a price ending in .50.

**Cause:** The total was being added up with plain floating point and
never rounded, so 0.1 + 0.2 style errors showed up on screen. The
rounding helper existed but this one place was not using it.

**Fix:** Ran the total through the existing rounding helper instead of
adding a new one, so every price on screen goes through the same path.

**Checked:** Cart, checkout and the order screen all show Rs. 130 now.
Prices without decimals still show without a trailing.00.

**Time:** about 40 minutes, most of it working out that the cart and the
order screen round in different places.



## CC-01 — "The search suggestions are behind everything"

**Reproduced:** Typed a dish name in the search input on the menu page. The suggestions dropdown appeared, but items below the top item were covered by category tabs and menu elements underneath, making them unclickable.

**Cause:** `.search-wrap` had a `z-index` of `1`, creating a stacking context lower than `.cat-tabs` (`z-index: 40`) and `#menu` (`z-index: 2`). As a result, `.suggest-box` (even with `z-index: 100`) was trapped inside `.search-wrap`'s lower stacking context and painted underneath `.cat-tabs`.

**Fix:** Increased `.search-wrap`'s `z-index` to `41` in `frontend/style.css`, elevating `.search-wrap` and its suggestions dropdown above `.cat-tabs` (`z-index: 40`).

**Checked:** Searched for dish names; suggestions dropdown now renders cleanly above category tabs and dish grid cards, allowing all suggestion items to be clicked.

**Time:** about 20 minutes.




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
