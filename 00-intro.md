---
layout: page
title: Introduction to research computing on the Palmetto cluster
subtitle: Introducing the Palmetto cluster
minutes: 5
---

> ## Learning Objectives {.objectives}
>
>

## Nelle's Pipeline: Starting Point

Nelle Nemo, a marine biologist,
has just returned from a six-month survey of the
[North Pacific Gyre](http://en.wikipedia.org/wiki/North_Pacific_Gyre),
where she has been sampling gelatinous marine life in the
[Great Pacific Garbage Patch](http://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch).
She has 300 samples in all, and now needs to:

1.  Run each sample through an assay machine
    that will measure the relative abundance of 300 different proteins.
    The machine's output for a single sample is
    a file with one line for each protein.
2.  Calculate statistics for each of the proteins separately
    using a program her supervisor wrote called `goostat`.
3.  Compare the statistics for each protein
    with corresponding statistics for each other protein
    using a program one of the other graduate students wrote
    called `goodiff`.
4.  Write up results.
    Her supervisor would really like her to do this by the end of the month
    so that her paper can appear in an upcoming special issue of *Aquatic Goo Letters*.

It takes about half an hour for the assay machine to process each sample.
The good news is that
it only takes two minutes to set each one up.
Since her lab has eight assay machines that she can use in parallel,
this step will "only" take about two weeks.

The bad news is that if she has to run `goostat` and `goodiff` by hand,
she'll have to enter filenames and click "OK" 45,150 times
(300 runs of `goostat`, plus 300*299/2 (half of 300 times 299) runs of `goodiff`).
At 30 seconds each,
that will take more than two weeks.
Not only would she miss her paper deadline,
the chances of her typing all of those commands right are practically zero.

A small subset of Nelle's machine output files is available in
the directory `data-shell/north-pacific-gyre/2012-07-03`.
To compute the statistics for a given file,
she runs the `goostats` shell script:

~~~
bash goostats <filename> <output_filename>
~~~

This takes about 30 seconds to run for each file,
which means that for even this small subset of 17 files,
Nelle will have to wait at least about 8 minutes to see all the results.
Instead, she would like to use the Palmetto cluster to process several files
*in parallel*.
Because each file can be processed independently,
if she had 17 CPU cores working on a single file each,
her analysis would be complete in 30 seconds, rather than 8 minutes.
