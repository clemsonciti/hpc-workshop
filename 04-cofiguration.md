---
layout: lesson
title: Introduction to HPC on the Palmetto Cluster
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

Let's create our own executable,
and add it to the `PATH`.
Start by creating a directory called `scripts` in your home directory.

~~~{.bash}
$ mkdir scripts
$ cd scripts
~~~

In this directory,
create a script called `sayHello.sh`
with the following contents:

~~~
#!/bin/bash

echo Hello
~~~

To make the script executable,
you have to change the permissions on it:

~~~{.bash}
$ chmod u+x sayHello.sh
~~~

From the same directory,
you can execute the script easily
by providing the path to the script:

~~~{.bash}
$ ./sayHello.sh
~~~

~~~{.output}
Hello
~~~

But note that you cannot do this from anywhere else,
for example from your home directory:

~~~{.bash}
$ cd
$ ./sayHello.sh
~~~

~~~{.error}
-bash: ./sayHello.sh: No such file or directory
~~~

You have to provide the full path to the script:

~~~{.bash}
$ scipts/sayHello.sh
~~~

Providing the full path for each executable we run
can be tedious.
To make the executable accessible from anywhere,
we'll have to append the `scripts` directory to the `PATH`:

~~~{.bash}
$ export PATH=$PATH:/home/username/scripts
~~~

In the command above, we're replacing the value
of the `PATH` variable
with it's current value and then *appending*
the new directory `/home/username/scripts`.
Notice that we use the `:` in between,
because that's how `PATH` expects directories to be separated.
If you log out and log back in to the Palmetto, however,
the `PATH` variable retains its old value.

### The `.bashrc` file

`.bashrc` is a file in your home directory
that contains a list of commands for bash to run
when it starts (the `rc` stands for run commands).
Let's add the `scripts` directory to the `PATH` in the `.bashrc`
file:

~~~
#.bashrc

export PATH=$PATH:/home/username/scripts
~~~

Let's now try to run `sayHello.sh`

~~~{.bash}
$ sayHello.sh
~~~

~~~{.output}
Hello
~~~


