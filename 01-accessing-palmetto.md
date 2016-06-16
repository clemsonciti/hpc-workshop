---
layout: lesson
title: Introduction to research computing on the Palmetto cluster
subtitle: First steps
minutes: 45
---

> ## Learning objectives {.objectives}
> * Learn how to log in to the Palmetto cluster.
> * Learn the basic structure of the Palmetto cluster.

In this workshop,
we will use a command-line interface to interact with
the Palmetto cluster, which runs the Linux operating system
specifically, [Scientific Linux](https://www.scientificlinux.org/).
However, note that these commands can be used on
any *Unix-based* operating system,
including Mac OS X.

To be able to run commands on the Palmetto from your own machine,
you will first need to be able to log in to the Palmetto.
This is known as a *remote login*.
If you run Mac OS X or any other Unix-based operating
system on your machine,
you can log in remotely by opening a terminal
and using the `ssh` command:

~~~{.bash}
$ ssh username@user.palmetto.clemson.edu
~~~

If you run Windows,
you will use the SSH Secure Shell to log in.
Click on  `File > Quick Connect`,
and use the following parameters (whichever required):

* Host name: `user.palmetto.clemson.edu`  
* User name: Clemson username   
* Port number: 22  
* Authentication method: none specified

When logged in,
you are presented with a welcome message
and the following "prompt":

~~~{.bash}
[username@user001 ~]$ 
~~~

The prompt in a bash shell usually
consists of a dollar (`$`) sign,
and shows that the shell is waiting for input.
The prompt may also contain other information:
this prompt tells you your username and which node
you are connected to -
`user001` is the "login" node.
It also tells you your current directory,
i.e., `~`, which, as you will learn shortly,
is short for your *home* directory.
We will mostly refer to the prompt as just `$`, i.e.,

~~~{.bash}
$ 
~~~

## Structure of the cluster

<img src="fig/palmetto-structure.png" \
     alt="Structure of the Palmetto cluster" \
     style="height:350px">

The Palmetto cluster has several "compute" nodes
that can perform fast calculations on large amounts of data.
To see what kind of compute nodes are available
and which ones are free, use the `whatsfree` command:

~~~{.bash}
$ whatsfree
~~~

~~~{.output}
TOTAL NODES: 2021     NODES FREE: 743     NODES OFFLINE: 79     NODES RESERVED: 58

PHASE 0    TOTAL =   6  FREE =   3  OFFLINE =   1  TYPE = HP bigmem machines with 24 or 64 cores, and 504GB or 2TB
PHASE 1    TOTAL = 233  FREE = 106  OFFLINE =  33  TYPE = Dell   PE1950  Intel Xeon  E5345,      8 cores,  12GB, mx
PHASE 2    TOTAL = 244  FREE =  79  OFFLINE =   0  TYPE = Dell   PE1950  Intel Xeon  E5410,      8 cores,  12GB, mx
PHASE 3    TOTAL = 244  FREE = 145  OFFLINE =   5  TYPE = Sun    X2200   AMD Opteron 2356,       8 cores,  16GB, mx
PHASE 4    TOTAL = 330  FREE = 239  OFFLINE =   3  TYPE = IBM    DX340   Intel Xeon  E5410,      8 cores,  16GB, mx
PHASE 5a   TOTAL = 374  FREE = 162  OFFLINE =   1  TYPE = Sun    X6250   Intel Xeon  L5420,      8 cores,  32GB, mx
PHASE 5b   TOTAL =   9  FREE =   9  OFFLINE =   0  TYPE = Sun    X4150   Intel Xeon  E5410,      8 cores,  16GB, mx
PHASE 6    TOTAL =  69  FREE =   0  OFFLINE =   1  TYPE = HP     DL165   AMD Opteron 6176,      24 cores,  48GB, mx
PHASE 7a   TOTAL =  42  FREE =   0  OFFLINE =  22  TYPE = HP     SL230   Intel Xeon  E5-2665,   16 cores,  64GB, FDR
PHASE 7b   TOTAL =  12  FREE =   0  OFFLINE =   1  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  64GB, FDR, M2075
PHASE 8a   TOTAL =  77  FREE =   0  OFFLINE =   1  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  64GB, FDR, K20, SSD
PHASE 8b   TOTAL =  51  FREE =   0  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores,  64GB, FDR, K20
PHASE 8c   TOTAL =  68  FREE =   0  OFFLINE =   1  TYPE = Dell   PEC6220 Intel Xeon  E5-2665,   16 cores,  64GB, QDR, 10ge
PHASE 9    TOTAL =  72  FREE =   0  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2665,   16 cores, 128GB, FDR, K20, 10ge
PHASE 10   TOTAL =  80  FREE =   0  OFFLINE =   3  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 128GB, FDR, K20, 10ge
PHASE 11a  TOTAL =  40  FREE =   0  OFFLINE =   3  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 128GB, FDR, K40, 10ge
PHASE 11b  TOTAL =   4  FREE =   0  OFFLINE =   0  TYPE = HP     SL250s  Intel Xeon  E5-2670v2, 20 cores, 128GB, FDR, Phi, 10ge
PHASE 12   TOTAL =  30  FREE =   0  OFFLINE =   3  TYPE = Lenovo NX360M5 Intel Xeon  E5-2680v3, 24 cores, 128GB, FDR, K40, 10ge
PHASE 13   TOTAL =  24  FREE =   0  OFFLINE =   0  TYPE = Dell   C4130   Intel Xeon  E5-2680v3, 24 cores, 128GB, FDR, K40, 10ge
PHASE 14   TOTAL =  12  FREE =   0  OFFLINE =   1  TYPE = HP     XL190r  Intel Xeon  E5-2680v3, 24 cores, 128GB, FDR, K40, 10ge
~~~

It also has a few so-called "service" nodes,
that are *not* meant for computation.
Instead, they are meant to help users perform other actions
such as transfering code and data to and from the cluster.

The most important of these "service" nodes is
the login node `user001`.
The login node runs a "server" program
that listens for remote logins.
On our own machines, we run a "client" program
(Secure Shell or `ssh`) that can talk to this server.
Our client program passes our login credentials to this server,
and if we are allowed to log in,
the server runs a shell for us on the computer
it is running on (`user0001`).
Any commands that we enter into this shell
are executed not by our own machines,
but by `user0001`.

Another important service node on the Palmetto cluster
is the *scheduler* node.
The scheduler decides which
users can run their computations on the cluster and when.
Different HPC clusters use different schedulers,
which may be suited for different kinds of computations - currently, we run
the PBS Professional (version 13.1) scheduler on the cluster.

### Storage

There are three primary locations on the palmetto cluster
that you can store code and data:

1. Your home directory (`/home/username`). This is where
you are placed when you log in to the cluster.
You can have up to 100 GB of backed-up data here.
Use the `checkquota` command to see how much space you are using:

~~~{.bash}
$ checkquota
~~~

~~~{.output}
/home/username:       23.6 GB of 100.0 GB; or 416,085 of 1,000,000 files
~~~

2. Your scratch directories (`/scratch1/username` and `/scratch2/username`).
Here, you can store virtually unlimited data - but it won't be backed up,
and can disappear at any time.
As a matter of policy, files untouched for more than
30 days will be deleted. The scratch directories are meant to be
temporary "work" spaces for your computations.
