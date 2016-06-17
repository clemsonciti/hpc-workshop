---
layout: lesson
title: Introduction to research computing on the Palmetto cluster
subtitle: Running your first job
minutes: 20
---

> ## Learning objectives {.objectives}
> 
> * Distinguish between the login node and compute nodes
> * Run your first interactive job on the Palmetto cluster
> * Learn how to load modules

The Palmetto cluster consists of several hundred "compute" nodes,
each equipped with CPUs and GPUs to do heavy computation.
The login node `user001` is a "service" node,
shared by all users,
and is meant only for tasks such as managing projects
(editing and moving files and directories),
and submitting batch scripts (about which we will learn shortly).

We'd like to start analyzing the data we've downloaded.
So let's get access to a compute node to begin:

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

Your home directory (and all the files inside it)
are still accessible from the compute nodes:

~~~{.bash}
$ ls
~~~

~~~{.output}
data-hpc data-hpc.zip
~~~

Let's navigate to the directory where our data files are:

~~~{.bash}
$ cd data-hpc/north-pacific-gyre/2012-07-03/
$ ls
~~~

~~~{.output}
goostats        NENE01736A.csv  NENE01812A.csv  NENE01971Z.csv  NENE02018B.csv  NENE02040Z.csv  plots
NENE01729A.csv  NENE01751A.csv  NENE01843A.csv  NENE01978A.csv  NENE02040A.csv  NENE02043A.csv  results
NENE01729B.csv  NENE01751B.csv  NENE01843B.csv  NENE01978B.csv  NENE02040B.csv  NENE02043B.csv  stats.py
~~~

The script we have to analyze our file is called `stats.py`.
It's a Python script, and needs specific Python modules to run.
Try:

~~~{.bash}
$ python stats.py
~~~{.output}

And you should see an error about missing modules.

"Missing software" is a common problem when working with HPC clusters
such as Palmetto. Software that you are used to having/using on your personal
machines are generally missing from the cluster. Several commonly used software
packages are installed on the cluster - however, you will manually have to
"load" them. The specific software package we need is
called `anaconda`. So let's load that package:

~~~{.bash}
$ module load anaconda/2.5.0
~~~

You can see a list of software packages that can be loaded in this way
by using `module avail`:

~~~{.bash}
$ module avail
~~~

~~~{.output}
------------------------------------------------------------------- /software/modulefiles --------------------------------------------------------------------
abaqus/6.10                    cufflinks/2.1.1                gromacs/5.0.5-gpu              mpich2/1.4                     opensees/2.4.6
abaqus/6.13                    cusp/0.5.1                     gromacs/5.0.5-nogpu            namd/20150610                  openvswitch/2.4.0
abaqus/6.14                    divvy/0.9                      gsl/1.15                       namd/20150610-intel            paraview/4.1.0
amber/14                       dmol/3.0                       gsl/1.16                       namd/2.11b1                    paraview/5.0
anaconda/1.9.1                 earlang/18.2.1                 gurobi/6.5.0                   namd/2.9                       postgis/2.2.1
anaconda/2.3.0                 emboss/6.6                     hadoop/1.2.1                   namd/2.9-k20                   postgresql/9.4.0
anaconda/2.4.0                 espresso/5.1                   hdf5/1.10.0                    netcdf/4.3.3.1                 python/2.7.6
anaconda/2.5.0                 fastqc/0.10.1                  hdf5/1.8.15                    netcdf/4.4.0                   python/3.3.3
anaconda/4.0.0                 ffmpeg/2.4                     houdini/13.0.547               netcdf-parallel/1.6.1          python/3.4
anaconda3/2.5.0                fftw/3.3.3-dp-gcc              hwloc/1.10.1                   nvbio/20150213                 qemu/1.7.0
.
.
.
~~~

You can have more than one package loaded, but take care not to load
conflicting packages. Some software is *licensed* and can only
be used by a limited number of users at a given time. If you don't
see a software package that you want to use listed above,
then you will have to install and configure that software for yourself.
You can learn more about this from the Palmetto User's Guide.

Now that you've loaded the `anaconda` module, you can run
`stats.py`. Luckily, `stats.py` has some documentation that you
can access using the `-h` switch:

~~~{.bash}
$ python stats.py -h
~~~

~~~{.output}
usage: stats.py [-h] [--fname FNAME]

Script to analyze assay machine output. For a given input file, this script
computes the least abundant and most abundant protein. It also plots the
relative abundance of all proteins. The statistics are put in the results/
directory and the plot is put in the plots/ directory.

optional arguments:
  -h, --help     show this help message and exit
    --fname FNAME  Input file name
~~~

So, to analyze the file `NENE01729A.csv` using `stats.py`,
we can run the following command:

~~~{.bash}
$ python stats.py --fname NENE01729A.csv
~~~

This takes a while, but after the script is done,
you should see two things:

1. A results file called `NENE01729A-results.txt` in the `results/`
directory.
2. A plot called `NENE01729A.png` in the `plots/` directory.

You can copy these files back to your local machine
in the same way you copied files *in* to the cluster.
Mac OS X/Linux users will use `scp`:

~~~{.bash}
$ scp -r atrikut@user.palmetto.clemson.edu:/home/username/data-hpc/north-pacific-gyre/2012-07-03/plots/ .
~~~

Windows user's can use the file transfer client as before.

You'll notice that running the `stats.py` script took a while
(about a minute). Let's say we had a few hundred files to analyze,
even with a loop to automate the analysis, we would have to wait
at least a few hours for our results.
Later in the lesson, we'll learn how we can bring this time down to a
few minutes.
