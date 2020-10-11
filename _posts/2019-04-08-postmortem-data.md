---
title: "Postmortem of a data analysis mistake"
categories: data-science
date: 2019-04-08
tags:
  - data engineering
  - analysis
  - postmortem
  - sre
---

Making mistakes is something that will happen, no matter what, no matter how many guarantees, no matter how well prepared or designed a plan or a system can be, no matter how experienced is the professional. You will make a mistake. With that in mind, it's easier to plan for mistakes and try to take them into account when planning.

![Olho Vivo e Faro Fino](http://infantv.com.br/infantv/wp-content/uploads/2016/06/xvy3exd8.jpg)


And understanding the mistake is fundamental for trying your best to not make it again. With that in mind, I've been discussing lately about SRE, Software Reliability Engineering. It can be applied as an intersection of putting machine learning products into production and reproducibility, a concept that I consider extremely relevant for data science and science in general.

I was tasked on doing a data analysis directed to anomaly detection on a large data set. But this time, instead of using a pipeline that I'm more used to, I had to use a constrained analysis platform, for motives that are not important. What's relevant is that I couldn't use Spark or Pandas directly; the data set was in partitioned Parquet files, with each partition larger than the amount of RAM that I had available. Being outside my comfort zone should have kept me on my toes, but instead, I rushed, wanting to get to the data as soon as possible, to understand it. And that was my big mistake.

Because of the number of transformations that I had available and because of the structure of the data, I didn't do the most basic thing that a data scientist should do when dealing with a new data set: check it. Double check it. Search for missing values, for weird values, try to make sense on it, BEFORE doing any analysis. If a graph is created, make sure that you question every single detail of that graph, the units, the anomalies (or lack thereof!), and so on. Cut the data into smaller samples and check them. Don't assume anything and make all efforts possible to check ideas about the problem. *Just look at the data and see if makes any sense*. Don't explain anything before checking whatever point several times.

How many times I told that to my juniors, to my trainees? *Check your data, leave your preconceived ideas at the door.* And I did the same nonetheless. Lesson (hopefully) learned: never trust data before checking from different angles and really trusting that it is what it is. 