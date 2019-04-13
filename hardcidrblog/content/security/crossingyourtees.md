---
title: "Crossing Your Tees: Bug Hunting for Fashion"
menuTitle: "Bonobos Security"
date: 2019-04-09T12:57:45-04:00
draft: false
---

*Note: Bonobos has been contacted about this issue and has been taken care of. All of the codes are dead now anyway :P* 

Every year for the past couple of years the online retailer Bonobos.com has held an "Easter Egg Hunt" on their website. This is typically a fun event whereby you check various pages on their website for codes used to unlock store credit, discounts, and other goodies. The main prize this year was a $500 voucher off any order, but you had to be the first to find and use the code. I woke up fairly for a Saturday (10AM) to participate in this competition, figuring that I could always use some new Bonobos clothes on the cheap. I noticed that they changed it up this year - instead of plaintext codes, they have a javascript popup that displays an image containing the code. Clever, probably harder to download the entire site and grep your way to victory this year. Below is what you see when you first get to the site:

![Screen-Shot-2013-03-30-at-2_45_56-PM_qvbesb](/Screen-Shot-2013-03-30-at-2_45_56-PM_qvbesb.png)

Interesting, let's take a look at the source just for curiosity's sake.

![Screen-Shot-2013-03-30-at-2_51_45-PM_a2fhtt](/Screen-Shot-2013-03-30-at-2_51_45-PM_a2fhtt.png)

So it links directly to an image! After playing around with the URL, I found that directory listing/traversal wasn't an option. Well, what happens when we increment/decrement the image file name?

![Screen-Shot-2013-03-30-at-2_57_27-PM_iwzwmh](/Screen-Shot-2013-03-30-at-2_57_27-PM_iwzwmh.png)

Awesome, it looks like they are using sequential filenames for this promotion. After decrementing further, I was able to score all of the coupon codes with ease. Here they are below:

![Screen-Shot-2013-03-30-at-2_55_44-PM_ffveon](/Screen-Shot-2013-03-30-at-2_55_44-PM_ffveon.png)

![Screen-Shot-2013-03-30-at-2_57_27-PM_iwzwmh--1-](/Screen-Shot-2013-03-30-at-2_57_27-PM_iwzwmh--1-.png)


![Screen-Shot-2013-03-30-at-2_57_38-PM_p1txyt](/Screen-Shot-2013-03-30-at-2_57_38-PM_p1txyt.png)

![Screen-Shot-2013-03-30-at-2_57_45-PM_rsawbn](/Screen-Shot-2013-03-30-at-2_57_45-PM_rsawbn.png)

Instead of abusing this newfound power, I decided to call Bonobos and report the issue directly to them. They answered the phone immediately (amazing customer service), and I reported the issue to the rep, who was able to replicate the problem and report it to someone who could fix it. He said that the $500 code was already taken, but I could use any of the ones that I discovered (all of them). I tried all of them, and settled with the largest one that worked - the $100 off $250+. Not bad! +1 for responsible disclosure and Bonobos customer service! This issue shows how important it is to cross your tees (pun intended) and dot your i's when it comes to security. It's a minor issue, and it's definitely not as severe as an XSS or SQLInjection, but it could damage the reputation of the company. Simply randomizing that string of numbers would be enough to make it more secure, but they didn't go that far when setting it up. It may be the fault of Monetate.net, which is where the images are hosted, but there should have been some check along the line before launching the promotion.