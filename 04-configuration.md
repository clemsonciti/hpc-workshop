---
layout: lesson
title: Introduction to research computing on the Palmetto cluster
subtitle: Configuration and `.bashrc`
minutes: 20
---

> ## Learning objectives {.objectives}
> * Understand how to define and export variables
> * Understand the concept of environment variables
> * Understand where the shell looks for things
> * Edit the `.bashrc` to configure the shell and modify the environment

Let's create a directory called `thesis` and create a file there:

~~~{.bash}
$ cd
$ mkdir thesis
$ touch ~/thesis/draft.txt
~~~

To access this directory `thesis` from *anywhere in the filesystem*,
we have to use the absolute path to it:

~~~{.bash}
$ cd /home/username/thesis
~~~

If we (or our programs) are going to be accessing the directory a lot,
this can be inconvenient.
Fortunately, the shell let's us define *variables*
that can make it easier.
Let's create a variable that refers to this directory:

~~~{.bash}
$ THESIS_DIR=/home/username/thesis
~~~

Note that the equal to sign (`=`)
cannot have spaces to its left or right.
Now, we can simply use the variable to
access the directory:

~~~{.bash}
$ cd $THESIS_DIR
$ ls
~~~

~~~{.output}
draft.txt
~~~

Of course, variables don't have to refer to just
paths, they can refer to anything:

~~~{.bash}
$ NUM=4
$ echo $NUM
~~~

~~~{.output}
4
~~~

Let's create a simple bash script that tries to print
this value of NUM:

~~~
# script printNum.sh

echo $NUM
~~~

~~~{.bash}
$ bash printNum.sh
~~~

~~~{.output}
~~~

Oops. It doesn't look like the script knows
about this variable yet. To make the variable
accessible to the script,
we'll have to *export* the variable:

~~~{.bash}
export NUM=4
~~~

~~~{.bash}
$ bash printNum.sh
~~~

~~~{.output}
4
~~~

When you *export* a variable,
it gets added to the shell's *environment*.
Variables in the environment are accessible to other programs.
You can have a look at all the current environment variables using
the `env` command:

~~~{.bash}
$ env
~~~

~~~{.output}
MANPATH=:
HOSTNAME=user001
TERM=xterm-256color
SHELL=/bin/bash
HISTSIZE=1000
SSH_CLIENT=198.21.209.20 63708 22
.
.
.
~~~

If you look in the list, you will find the `NUM` variable
we exported earlier too.

Environment variables are a convenient way to configure programs.
For example, the bash shell uses the environment variable
`PS1` to decide what the prompt should look like.
Let's change the variable to see this in action:

~~~{.bash}
$ export PS1='>> '
~~~

~~~{.output}
>>
~~~

The prompt is now changed to `>>`
(to return back to your previous prompt, you have
to log out and log back in).

Although `PS1` is not an environment variable
that you may want to change,
it's a good example of how programs use environment
variables to configure their behaviour.

### The PATH environment variable

An important environment variable is `PATH`.
Let's have a look at it:

~~~{.bash}
$ echo $PATH
~~~

~~~{.output}
/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/Library/TeX/texbin
~~~

The output is a list of locations,
separated by the `:` character.
These are locations on the filesystem
that the shell will look for executables.
When you type in, for example,
the `whatsfree` command - it looks
in each of the above locations for an executable
called `whatsfree`.
If it finds it in any of those locations,
it executes it,
and if not, it gives you an error.
To confirm that the `whatsfree` executable is actually in
one of the above locations, you can use the `which` command:

~~~{.bash}
which whatsfree
~~~

You see that `whatsfree` is in the directory `/usr/bin`,
which is included in the `PATH`.

## Installing software

As an example of installing software, we will
install the [Hello](https://www.gnu.org/software/hello/)
package. We start by downloading and unpacking
the software:

~~~{.bash}
$ wget http://ftp.gnu.org/gnu/hello/hello-2.10.tar.gz
~~~~

~~~{.output}
--2016-06-17 05:29:15--  http://ftp.gnu.org/gnu/hello/hello-2.10.tar.gz
Resolving ftp.gnu.org... 208.118.235.20, 2001:4830:134:3::b
Connecting to ftp.gnu.org|208.118.235.20|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 725946 (709K) [application/x-gzip]
Saving to: 'hello-2.10.tar.gz'

hello-2.10.tar.gz                       100%[=============================================================================>] 708.93K  1.11MB/s    in 0.6s

2016-06-17 05:29:16 (1.11 MB/s) - 'hello-2.10.tar.gz' saved [725946/725946]
~~~

Once downloaded, we'll have to unpack it. This `.tar.gz` file is a popular
format on unix systems, and needs to be unpacked using the `tar` command:

~~~{.bash}
$ tar -xvf hello-2.10.tar.gz
~~~

~~~{.output}
x hello-2.10/
x hello-2.10/COPYING
x hello-2.10/tests/
x hello-2.10/tests/greeting-1
x hello-2.10/tests/traditional-1
x hello-2.10/tests/greeting-2
x hello-2.10/tests/hello-1
x hello-2.10/tests/last-1
x hello-2.10/Makefile.am
x hello-2.10/config.in
x hello-2.10/maint.mk
x hello-2.10/README
x hello-2.10/INSTALL
x hello-2.10/NEWS
x hello-2.10/GNUmakefile
.
.
.
~~~

To begin installing the package, let's `cd` into the
newly created `hello-2.10` directory,
and have look at the `INSTALL` file:

~~~{.bash}
$ cd hello-2.10
$ less INSTALL
~~~
