---
layout: lesson
title: Introduction to research computing on the Palmetto cluster
subtitle: Transferring files to/from the cluster
minutes: 20
---

> ## Learning objectives {.objectives}
> * Learn how to transfer files
>   to or from the cluster
> * Learn how to download data from
>   the web

Let's start by logging in to the cluster and having a look around:

~~~{.bash}
$ pwd
~~~

~~~{.output}
/home/username
~~~

If this is your first time logging in to the cluster,
this directory `/home/username` will be empty:

~~~{.bash}
$ ls
~~~

Let's populate the directory with some data.
The data from Nelle's experiments is available online
at the following location:

~~~
https://github.com/clemsoncoe/hpc-workshop/blob/gh-pages/data-palmetto.zip
~~~

To download this `.zip` file to your Desktop:

1. Click on the "Raw" button - this downloads the `.zip` file (presumably to your downloads directory).
2. Move the `.zip` file to the Desktop.

Now, we'd like to transfer this
file to our home directory on the Palmetto cluster.
How we do this depends on what operating system we run on our local machines.

## Using `scp` (Mac OS X)

Mac OS X/Linux systems provide a command-line utility called
`scp` that is used to transfer files to and from remote computers.
The basic form of an `scp` command to transfer a file from the local
machine to a remote computer is:

~~~{.bash}
$ scp /path/to/source/file user@hostname:/path/to/destination/directory
~~~

Thus, to copy the file `data-palmetto.zip` from your local machine's Desktop
to your home directory on the Palmetto, you can do the following:

~~~{.bash}
$ scp ~/Desktop/data-palmetto.zip username@user.palmetto.clemson.edu:/home/username/
~~~

## Using a file transfer client (Windows)

On Windows machines, a simple way to transfer files between
your local machine and the Palmetto cluster is via
the SSH client.
Simply click on the file transfer button as shown below.
This splits the screen in two halves,
so that you can drag files in your local machines
and copy them over to locations on the cluster:

<img src="fig/windows-filetransfer.png" \
     alt="Structure of the Palmetto cluster" \
     style="height:500px">

Once you've downloaded the data,
you can check the contents of your home directory:

~~~{.bash}
$ ls
~~~

~~~{.output}
data-palmetto.zip
~~~

You can use `unzip` to extract the contents of this `.zip` file:

~~~{.bash}
$ unzip data-palmetto.zip
~~~

~~~{.output}
Archive:  data-palmetto.zip
   creating: data-palmetto/
  inflating: data-palmetto/.bash_profile
  .
  .
  .
~~~

If the data is successfully unzipped, you should see a new *directory* in your
home directory:

~~~{.bash}
$ ls
~~~

~~~{.output}
data-palmetto data-palmetto.zip
~~~
