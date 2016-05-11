---
layout: page
title: Introduction to research computing on the Palmetto cluster
subtitle: Introducing the Palmetto cluster
minutes: 5
---

> ## Learning Objectives {.objectives}
>
> * Motivation for using the cluster.

## Nelle's Pipeline: Starting Point

Nelle Nemo, a marine biologist,
has returned from a six-month survey of the
[North Pacific Gyre](http://en.wikipedia.org/wiki/North_Pacific_Gyre),
where she has been sampling gelatinous marine life in the
[Great Pacific Garbage Patch](http://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
She collected 10,000 samples in all, and has run each sample through an assay machine
that measures the abundance of 300 different proteins.
The machine's output for a single sample is
a file with one line for each protein.
For example, the file `NENE01812A.txt` looks like this:

~~~
72:0.142961371327
265:0.452337146655
279:0.332503761597
25:0.557135549292
207:0.55632965303
107:0.96031076351
124:0.662827329632
193:0.814807235075
32:1.82402616061
99:0.7060230697
.
.
.
.
200:1.13383446523
264:0.552846392611
268:0.0767025200665
~~~

Each line consists of two "fields", separated by a colon (`:`).
The first field identifies the protein,
and the second field is a measure of the amount of protein in the sample.
Nelle needs to accomplish tasks such as the following:

1.  For any given file, extract the amount of a given protein.
2.  Find the maximum amount of a given protein across all files.
3.  For each file, run a program called `stats.py` that she wrote,
    which produces a graph, and writes some statistics
    (these go in the directories `plots/` and `results/` respectively.)

Each file takes about a minute to analyze.
In this lesson,
we'll see how Nelle can use the cluster to perform the above task faster
utilizing her campus HPC resource.
