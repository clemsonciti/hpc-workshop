---
layout: lesson
title: Introduction to research computing on the Palmetto cluster
subtitle: Submitting batch jobs
minutes: 20
---

> ## Learning objectives {.objectives}
> * Understand the difference between an interactive job
>   and a batch job
> * Understand the different components of a batch script
> * Submit a batch job using a submmission script
> * Learn about queues and priority
> * Learn how to monitor the progress of running jobs/
>   kill running jobs

So far, we have been using the cluster "interactively",
i.e., by typing in commands into a shell running on
the login node or the compute nodes.
This approach is good when setting up software,
running small experiments, and debugging/testing code - but
isn't the best use of our (or the cluster's!) time.
First, if the cluster is especially busy, it might
take a long time for our interactive session to begin
(the scheduler must wait for the resources to free up).
Second, we must be logged into the interactive session
for as long as our job takes. If we log out of the session,
or if connectivity to the cluster is lost,
the job is killed and any running programs are immediately stopped.

Instead, what we'd *like* to do is
let the scheduler know where our code and data is,
what resources we need to compute with them,
and whenever these resources are available,
to run our code and keep the results ready for us the next time we log in.

To do this, we must write a special script for the scheduler
called a batch submission script (or simply, a batch script).
A batch script is a shell script - so it consists mostly of shell commands.
In addition, it has a few lines with instructions for the scheduler - all of
these lines begin with `#PBS`.

Here's a simple submit script, called `sleep.sh`:

~~~{.bash}
#PBS -N sleeper
#PBS -l select=1:ncpus=1:mem=3gb,walltime=00:10:00

echo Starting job
sleep 60
echo Job finished
~~~

This batch script can be submitted to the scheduler
(only from the login node) using the following command:

~~~{.bash}
$ qsub sleep.sh
~~~

~~~{.output}
4442294.pbs02
~~~

When the job is submitted, we receive a job ID,
which we can use to query the status of the job:

~~~{.bash}
$ qstat 4442294
~~~

~~~{.output}
Job id            Name             User              Time Use S Queue
----------------  ---------------- ----------------  -------- - -----
4442295.pbs02     sleeper          atrikut           00:00:00 R c1_solo
~~~

The status (`S`) column shows that our job is running (`R`).
Jobs can have running (`R`), queued (`Q`), or error (`E`) status.

Once your job is finished, you will see the following output for `qstat`:

~~~{.output}
qstat: 4442295.pbs02 Job has finished, use -x or -H to obtain historical job information
~~~

And two new files in
the working directory:

~~~{.bash}
$ ls
~~~

~~~{.output}
sleeper.e4442295    sleeper.o4442295    sleep.sh
~~~

These files contain the error (`e`) and output (`o`) produced by the job.
Let's have a look at the output file:

~~~{.bash}
$ cat sleeper.o4442295
~~~

~~~{.output}
Starting job
Job finished


+------------------------------------------+
| PALMETTO CLUSTER PBS RESOURCES REQUESTED |
+------------------------------------------+

mem=1gb,ncpus=1,walltime=00:30:00


+-------------------------------------+
| PALMETTO CLUSTER PBS RESOURCES USED |
+-------------------------------------+

cpupercent=0,cput=00:00:00,mem=2880kb,ncpus=1,vmem=317320kb,walltime=00:01:01
~~~

~~~{.bash}
$ cat sleeper.e4442295
~~~

~~~{.output}

~~~

Example of job scripts for running a variety of types of jobs
is available [here](https://github.com/clemsonciti/palmetto-examples).
