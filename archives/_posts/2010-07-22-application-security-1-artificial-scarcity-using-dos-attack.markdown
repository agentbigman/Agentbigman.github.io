---
author: jagira
date: '2010-07-22 16:22:00'
layout: post
slug: application-security-1-artificial-scarcity-using-dos-attack
status: publish
title: 'Application Security #1 - Artificial Scarcity Using DoS Attack'
tags:
- security
---

Most E-Commerce or online booking websites lock the item/product
during the purchase process. \[after the customer selects the item
and proceeds for payment processing\]. This can be exploited by
launching a massive attack on the site in which bots/scripts
pretend to buy a product/item and drops out during payment process,
thereby locking all the items and creating an artificial
scarcity. 

**Locking**  

Websites have to lock the product/item during the purchase process
to prevent multiple purchase of the same item by different people.

**How to exploit it?**  

Create a script that will choose a product, provide fake details
and proceed to payment processing. The object will be locked for a
specific time \[depending on the site\]. Repeat the process using
multiple computers until all of the products are locked. Exploiting
this scarcity depends on the scenario - a person might buy a
special ticket/product for his g/f, it might increase the price of
the product during the lock period, a person might buy all the
items....

**Defense** 

-   One purchase request per IP address.
-   Reduce the locking period.
-   Remind the user of the locking period during the process.
-   Check for notorious activity. \[too many hung purchases\]   

 

*This hack was discussed by Trey Ford (WhiteHat Security) during BlackHat '09.*



