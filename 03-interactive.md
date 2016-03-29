---
layout: lesson
title: Introduction to HPC on the Palmetto Cluster
subtitle: Scheduling an interactive job
minutes: 20
---

> ## Learning objectives {.objectives}
> * Schedule an interactive job on the Palmetto
> * Learn about the difference resources and limits

The Palmetto cluster consists of several hundred "compute" nodes,
each equipped with CPUs and GPUs to do heavy computation.
The login node `user001` is a "service" node,
shared by all users,
and is meant only for tasks such as managing projects
(editing and moving files and directories),
and submitting batch scripts (about which we will learn shortly).

The file `/etc/hardware-table` lists the resources
available on the Palmetto cluster.

~~~{.bash}
$ cat /etc/hardware-table
~~~

~~~{.output}
PALMETTO HARDWARE TABLE      Last updated:  October 2015

Type "whatsfree" to see what nodes are currently free.

PHASE COUNT  MAKE   MODEL    CHIP(0)                CORES  RAM(1)    /local_scratch   Interconnect    FLOP  GPUs  PHIs SSD
 0      5    HP     DL580    Intel Xeon    7542       24   505 GB(2)    99 GB         N/A              4     0     0    0
 0      1    HP     DL980    Intel Xeon    7560       64     2 TB(2)    99 GB         mx, 10g          4     0     0    0
 1    247    Dell   PE1950   Intel Xeon    E5345       8    12 GB       37 GB         mx, 10g          4     0     0    0
 2    245    Dell   PE1950   Intel Xeon    E5410       8    12 GB       37 GB         mx, 10g          4     0     0    0
 3    247    Sun    X2200    AMD   Opteron 2356        8    16 GB      193 GB         mx, 10g          4     0     0    0
 4    332    IBM    DX340    Intel Xeon    E5410       8    16 GB      111 GB         mx, 10g          4     0     0    0
 5a   383    Sun    X6250    Intel Xeon    L5420       8    32 GB       31 GB         mx, 10g          4     0     0    0
 5b     9    Sun    X4150    Intel Xeon    E5410       8    16 GB       99 GB         mx, 10g          4     0     0    0
 6     69    HP     DL165    AMD   Opteron 6176       24    48 GB      193 GB         mx, 10g          4     0     0    0
 7a    42    HP     SL230    Intel Xeon    E5-2665    16    64 GB      240 GB         fdr, 56g         8     0     0    0
 7b    12    HP     SL250s   Intel Xeon    E5-2665    16    64 GB      240 GB         fdr, 56g         8     2(3)  0    0
 8a    96    HP     SL250s   Intel Xeon    E5-2665    16    64 GB      900 GB         fdr, 56g         8     2(4)  0    300 GB(6)
 8b    32    HP     SL250s   Intel Xeon    E5-2665    16    64 GB      420 GB         fdr, 56g         8     2(4)  0    0
 8c    68    Dell   PEC6220  Intel Xeon    E5-2665    16    64 GB      350 GB         qdr, 10ge, 40g   8     0     0    0
 9     72    HP     SL250s   Intel Xeon    E5-2665    16   128 GB      420 GB         fdr, 10ge, 56g   8     2(4)  0    0
10     80    HP     SL250s   Intel Xeon    E5-2670v2  20   128 GB      800 GB         fdr, 10ge, 56g   8     2(4)  0    0
11a    40    HP     SL250s   Intel Xeon    E5-2670v2  20   128 GB      800 GB         fdr, 10ge, 56g   8     2(5)  0    0
11b     4    HP     SL250s   Intel Xeon    E5-2670v2  20   128 GB      800 GB         fdr, 10ge, 56g   8     0     2(7) 0
12     30    Lenovo NX360M5  Intel Xeon    E5-2680v3  24   128 GB      800 GB         fdr, 10ge, 56g  16     2(5)  0    0
13     24    Dell   C4130    Intel Xeon    E5-2680v3  24   128 GB      800 GB         fdr, 10ge, 56g  16     2(5)  0    0

TOTAL: 2021 nodes / 22336 cores

PBS resource requests are always lowercase

(0) CHIP has 3 resources:   chip_manufacturer, chip_model, chip_type
(1) Leave 2GB for the operating system when requesting memory in PBS jobs
(2) Specify queue "bigmem" to access the large memory machines
(3) 2 NVIDIA Tesla M2075 cards per node, use resource request "ngpus=[1|2]" and "gpu_model=m2075"
(4) 2 NVIDIA Tesla K20m  cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k20"
(5) 2 NVIDIA Tesla K40m  cards per node, use resource request "ngpus=[1|2]" and "gpu_model=k40"
(6) Use resource request "ssd=true" to request a chunk with SSD in location /ssd1, /ssd2, and /ssd3 (100GB max each)
(7) Use resource request "nphis=[1|2]" to request phi nodes, the model is Xeon 7120p
~~~

The Palmetto cluster is divided into several "phases",
with each "phase" consisting of compute nodes
equipped with specified hardware.
Within a phase, nodes can communicate data with each other
using specified "interconnect".

To see what nodes in which phases are free,
the `whatsfree` command is available:

~~~{.bash}
$ whatsfree
~~~

~~~{.output}
Mon Mar 28 2016 20:23:32

TOTAL NODES: 2021     NODES FREE: 551     NODES OFFLINE: 230     NODES RESERVED: 0

PHASE 0    TOTAL =   6  FREE =   0  OFFLINE =   1  TYPE = HP bigmem machines with 24 or 64 cores, and 504GB or 2TB
PHASE 1    TOTAL = 245  FREE =   0  OFFLINE =  28  TYPE = Dell   PE1950  Intel Xeon  E5345,      8 cores,  12GB, mx
PHASE 2    TOTAL = 244  FREE = 130  OFFLINE =  72  TYPE = Dell   PE1950  Intel Xeon  E5410,      8 cores,  12GB, mx
PHASE 3    TOTAL = 244  FREE =  44  OFFLINE =   3  TYPE = Sun    X2200   AMD Opteron 2356,       8 cores,  16GB, mx
PHASE 4    TOTAL = 330  FREE = 299  OFFLINE =   1  TYPE = IBM    DX340   Intel Xeon  E5410,      8 cores,  16GB, mx
PHASE 5a   TOTAL = 374  FREE =   0  OFFLINE =  40  TYPE = Sun    X6250   Intel Xeon  L5420,      8 cores,  32GB, mx
PHASE 5b   TOTAL =   9  FREE =   9  OFFLINE =   0  TYPE = Sun    X4150   Intel Xeon  E5410,      8 cores,  16GB, mx
PHASE 6    TOTAL =  69  FREE =  18  OFFLINE =   1  TYPE = HP     DL165   AMD Opteron 6176,      24 cores,  48GB, mx
PHASE 7a   TOTAL =  42  FREE =   0  OFFLINE =  42  TYPE = HP     SL230   Intel Xeon  E5-2665,   16 cores,  64GB, FDR
PHASE 7b   TOTAL =  12  FREE =   0  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  64GB, FDR, M2075
PHASE 8a   TOTAL =  81  FREE =   0  OFFLINE =   1  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  64GB, FDR, K20, SSD
PHASE 8b   TOTAL =  47  FREE =   0  OFFLINE =   3  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  64GB, FDR, K20
PHASE 8c   TOTAL =  68  FREE =  33  OFFLINE =  27  TYPE = Dell   PEC6220 Intel Xeon  E5-2665,   16 cores,  64GB, QDR, 10ge
PHASE 9    TOTAL =  72  FREE =   1  OFFLINE =   3  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores, 128GB, FDR, K20, 10ge
PHASE 10   TOTAL =  80  FREE =   3  OFFLINE =   5  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 128GB, FDR, K20, 10ge
PHASE 11a  TOTAL =  40  FREE =   9  OFFLINE =   1  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 128GB, FDR, K40, 10ge
PHASE 11b  TOTAL =   4  FREE =   2  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 128GB, FDR, Phi, 10ge
PHASE 12   TOTAL =  30  FREE =   3  OFFLINE =   0  TYPE = Lenovo NX360M5 Intel Xeon  E5-2680v3, 24 cores, 128GB, FDR, K40, 10ge
PHASE 13   TOTAL =  24  FREE =   0  OFFLINE =   2  TYPE = Dell   C4130   Intel Xeon  E5-2680v3, 24 cores, 128GB, FDR, K40, 10ge
~~~

Nodes that are "offline" are inacessible to users.

Because you're currently logged in to the
login node, all commands you type in to the shell
are executed there, not on any of the above nodes.
For your commands to be executed on the compute nodes instead,
you will first need to log in to a compute node:
You can do this with the `qsub -I` command:

~~~{.bash}
$ qsub -I
~~~

~~~{.output}
qsub (Warning): Interactive jobs will be treated as not rerunnable
qsub: waiting for job 2725837.pbs02 to start
qsub: job 2725837.pbs02 ready

[username@node1466 ~]$ 
~~~

The prompt indicates that you are no longer
running a shell on the login node (`user001`),
but a compute node (`node1466` or similar).
The `qsub` command is used to submit "jobs"
to the cluster.
A job is a request for cluster resources,
like hardware (like cpu cores, memory, interconnect)
and time (minutes or hours).
The `-I` flag in the `qsub` command stands
for "interactive".
This job is interactive because commands
are entered interactively into a
shell running on a compute node.
(This is opposed to a *batch* job,
in which the commands are included in a script---more
on that later).

Every job you run is associated with a unique job ID
(in the above example, this ID is 2825873).

By default,
`qsub` submits a request for
1 core on 1 compute node with
1 GB of RAM for 30 minutes.
You can request more (or less) of each of these resources
by modifying the `qsub` command.
For instance, to use 2 CPU cores for 10 minutes:

~~~{.bash}
$ qsub -I -l select=1:ncpus=2,walltime=00:10:00
~~~

Here, we use `select=1` to specify the number of
"chunks" of hardware that we'd like.
The following command requests 4 "chunks" of hardware,
with 2 CPU cores, 4 GB of RAM *each* - for 2 minutes:

~~~{.bash}
$ qsub -I -l select=4:ncpus=2:mem=4gb,walltime=00:02:00
~~~

When making requests for resources,
consult `/etc/hardware-table` and the `whatsfree` command.
Note that a "chunk" corresponds to all or part of a node.
So the number of resources per chunk cannot be inconsistent
with the resources actually available.
For example, the below request is invalid,
as nodes on phase 3 only have 8-core CPUs.

~~~{.bash}
$ qsub -I -l select=1:ncpus=16:mem=4gb:phase=3,walltime=00:02:00
~~~

When logged in to a compute node,
you can still edit and move files
in your home and other directories.

> ## Parallelization {.callout}
> We have made requests for resources like multiple cores per chunk
> and multiple chunks,
> but note that most programs cannot automatically take advantage
> of these resources.
> Programs written for a single CPU core will only ever
> execute on a single CPU core, regardless of how many cores are requested.
> To exploit multiple cores, programs must be written for parallel execution.
> We will learn how to do this later in the lesson.
