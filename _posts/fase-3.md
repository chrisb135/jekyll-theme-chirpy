---
title: An exploratory data analysis of the netdev mailing list in the Linux kernel
layout: post
date: 2026-07-01 14:00:00 -0300
description: This post describes the development and results of an exploratory data analysis performed on the netdev mailing list of the Linux kernel
---

## The idea for the analysis

Two of the classes before we were supposed to present our findings were dedicated to coming up with a research question in regards to one of the suggested datasets related to open source software. I gave an honest look to many of the datasets but, given that my research during my time at USP focuses on computer networks, it soon seemed all too natural to choose to analyse the netdev mailing list.

Doing so was relatively simple seeing as some colleagues had recently finished the LKML5Ws dataset (Linux Kernel Mailing List What, When, Who, Where, Why). This dataset included several interesting bits of information not directly related or not to different network technologies employed in the linux kernel, such as the volume of patches related to IPv4 relative to IPv6, what vendors have the highest average patch version per merge, as well as how much activity the eBPF subsystem gets in comparison to Netfilter.

## Network protocol patch volume comparison

My hypothesis for this part of the analysis was that, as IPv6 became more prevalent, patches containing changes pertaining to the protocol would also increase. In other words, I expected IPv6 patches to be more common in comparison to IPv4 patches in more recent years.

The data would prove otherwise, though, as the generated graph showed that the development for both protocols was largely proportional throughout the whole time period the dataset encapsulates. As one increases, so does the other, and vice-versa.

![protocol-comparison](assets/img/protocol-comparison.png)
_Comparison of network protocol patch volume per protocol_

This could be because IPv6 is not yet considered as viable to be the main protocol for the OS, so migration towards it is not yet a large focal point, but it could also indicate that most new features ship with both protocols in mind and is made to work with either, as well as the fact that even migration focused patches would still involve changes related to IPv4 that would be caught by the code I ran to generate this graph.

## Vendor upstream friction

Since the netdev mailing list has directories related to different vendors, it seems interesting to see how patch acceptance varies depending on the vendor.

![vendor-upstream](assets/img/vendor-upstream.png)
_Comparison of vendor upstream friction per vendor_

Results would show that variance is actually quite small. Patch version varied only between 1.5 and 2.

Something that was pointed out to me is that changes inside of a vendor's directory aren't necessarily made by that vendor, as the directories only refer to technology or hardware made originally by that vendor.

## Distribution of commit trailer tags

Originally, I was going to make a comparison on which vendors had more people test their patches and compare that to how many versions were needed for a patch to be merged. However, as it turns out, the tested-by trailer tag seemed very rare among the many commits of the dataset.

This gave me the idea to find out how much each trailer tag was used in the analysed commits. The generated graph would show that many of the more "collaborative" tags, such as tested-by or co-developed-by, are among the less used tags in the commits. An original version of the graph included every single trailer tag used, but there were way too many and several were just garbage, like my personal favorite: "untested-but-otherwise-signed-off-by". If the person reading this is the one that wrote that, I'm sorry, but I did find that very funny. I call it garbage for professional reasons only.

![trailer-tags](assets/img/trailer-tags.png)
_Ammount of trailer tags per tag_

This would give credence to the idea that Linux kernel developers largely work alone, but, during my presentation, it was brought to my attention that patches made by large companies may sign off patches with a common, "group-based" tag, or might have the team leader alone sign off the change, so sometimes even patches developed by many people would generate only one tag.

## Netfilter vs eBPF activity

Some of the key technologies whose development we can track through the mailing list are Netfilter and eBPF. Historically, Netfilter is the veteran solution for setting up firewalls. eBPF is a newer, broader technology that handles advanced networking and observability (and is increasingly used to replace or augment older Netfilter implementations in massive cloud and Kubernetes environments).

As such, my hypthesis for these frameworks was that, as eBPF is much more wide-spanning, and largely seen as easier to work with, it would show much heavier activity from the moment development on it started.

![netfilter-vs-eBPF](assets/img/legacy-vs-modern.png)
_Development of netfilter vs. eBPF_

This hypothesis would be proven correct by the results seen above. Netfilter falls behind as soon as eBPF comes into play.

However, this doesn't tell the whole story, as eBPF is much more wide-spanning, it is used in much more different contexts than Netfilter. Contrary to what I initially believed, even comparing the two is a bit disingenuous. At first, my view was that they were direct competitors, but in reality it's more of an "apples and oranges" comparison.

## Conclusion

Overall, this analysis showed many different directions more in-depth research could go, as the same time as it gave me valuable insight into the inner workings of Linux kernel network development and network development in general.
