---
title: Creating a patch for the IIO subsystem of the Linux Kernel
layout: post
date: 21-04-2026 14:00:00 -0300
description: This post details the development of a patch for the IIO subsystem of the Linux Kernel, aiming to replace `mutex_lock` and `unlock` calls with the newer `guard` variant
---

## Development of the patch

The idea for the patch came from a presentation given by the professors a few weeks prior in which suggestions were given on what areas of the Linux kernel would be good targets for a patch. Among those, the one that caught me and my partner, Eduardo's, eye were the many files where calls to `mutex_lock`, a function that prevents the use of critical variables by other processes while the current process uses them, and `mutex_unlock`, which signals that those same variables are now free for use, could be replaced by the new `guard` function, which does the same thing but consolidates those two calls into a single one that unlocks the variables as soon as the current scope is exited. This change is desired by Linux kernel devs and is seen as a modernization of the codebase. As it is also fairly simple, we picked it up for our patch.

Initially, my approach was extremely naive, simply replacing all calls to `mutex_lock` with `guard` and deleting calls to `mutex_unlock`. However, I admittedly began working on the patch quite close to the class's deadline for finishing it, so I didn't have much time to refine it. Regardless, initial tests (which consisted of simply compiling the kernel) indicated that I, at least, didn't break anything.

I did want to test it at least further than that, so I looked into ways to test my change given that it is somewhat reliant on physical hardware and I was working on a VM. I managed to make a crude CI pipeline for some simple tests after some trial and error and, it ran smoothly. Of course, unbeknownst to me, my professors already had a CI pipeline ready so I wasted a bunch of effort, but, oh well. With this, I felt confident enough to send the patch.

## Trying to send the patch

We were instructed to use our USP emails to send our patches to the maintainers. Doing this required an authentication process based on oauth which, in turn, depended on a list of emails which could use this authentification.

However, I quite simply did not know this, and so my name was not on this list. As such, I was incapable of sending my patch on the intended day. This may have been for the better though, as I would have been one of tens of nearly identical patches sent to the maintainers over something like a 2 week period, meaning it would almost assuredly have been heatedly rejected the way many of my colleagues's similar patches were.

In class, given this situation, I was told to wait a little before refining and sending my patch, so, as of yet, I haven't actually sent my patch in for review yet.
