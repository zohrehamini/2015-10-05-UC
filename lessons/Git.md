# Using Git & Github for version control

## Setting Up

The first time we use Git on a new machine,
we need to configure a few things.
Here's how:

~~~ {.bash}
$ git config --global user.name "Vlad Dracula"
$ git config --global user.email "vlad@tran.sylvan.ia"
$ git config --global color.ui "auto"
$ git config --global core.editor "nano -w"
~~~

*note 1 - if  you use other text editors, please replace the name of that with nano*

*note 2 - Please use your own name and email address instead of Dracula's*


## Creating a Repository

Once Git is configured,
we can start using it.
Let's create a directory for our work:

~~~
$ mkdir unique_extract
$ cd unique_extract
~~~

Let's tell Git that it should track this directory.

~~~
$ git init
~~~

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory called `.git`:

~~~ 
$ ls -a
~~~

Git stores information about the project in this special sub-directory.
If we ever delete it,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

~~~
$ git status
~~~


## Tracking Changes to Files

Let's create a file called `README.md` that contains some information about the project we are going to create. In sum, we are going to create a Shell script that will extract unique values from a column from a `.csv` file.

~~~ 
$ nano README.md
~~~

Type the text below into the `README.md` file:

~~~
# Extract Unique Elements 
~~~

`README.md` now contains a single line:

~~~ 
$ ls
~~~
~~~ 
README.md
~~~
~~~
$ cat README.md
~~~

If we check the status of our project again,
Git tells us that it's noticed the new file:

~~~ 
$ git status
~~~

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git that it should do so using `git add`:

~~~
$ git add README.md
~~~

and then check that the right thing happened:

~~~ 
$ git status
~~~

Git now knows that it's supposed to keep track of `README.md`,
but it hasn't yet recorded any changes for posterity as a commit.
To get it to do that,
we need to run one more command:

~~~ 
$ git commit -m "Added a Header to the README file"
~~~

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [revision](reference.html#revision)
and its short identifier is `f22b25e`.
(Your revision may have another identifier.)

We use the `-m` flag (for "message")
to record a comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured at the start)
so that we can write a longer message.

If we run `git status` now:

~~~ 
$ git status
~~~

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

~~~ 
$ git log
~~~
~~~ {.output}
commit f22b25e3233b4645dabd0d81e651fe074bd8e73b
Author: Vlad Dracula <vlad@tran.sylvan.ia>
Date:   Thu Aug 22 09:51:46 2013 -0400

    Added a Header to the README file
~~~

`git log` lists all revisions  made to a repository in reverse chronological order.
The listing for each revision includes
the revision's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the revision's author,
when it was created,
and the log message Git was given when the revision was created.


## Changing a File

Now lets adds more information to the file.
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)

~~~ 
$ nano README.md
$ cat README.md
~~~
~~~ 
# Extract Unique Elements
Tiffany Timbers, October 6, 2015
~~~

When we run `git status` now,
it tells us that a file it already knows about has been modified:

~~~ 
$ git status
~~~

The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
much less actually saved them (which we do with `git commit`).
So let's do that now. It is good practice to always review
our changes before saving them. We do this using `git diff`.
This shows us the differences between the current state
of the file and the most recently saved version:


Let's add and commit the changes!

~~~
git add README.md
git commit -m "Added authors name and the date"
git status
~~~

Let's add some more info to the file:

~~~
nano README.md
$ cat README.md
~~~
~~~ 
# Extract Unique Elements
Tiffany Timbers, October 6, 2015

This project is to develop a Bash Shell script.
~~~

Note - we have made changes and not yet added or committed them. We can use a command called `git diff` to compare this version of the file to the previous version:

~~~ 
$ git diff
~~~

The output is cryptic because
it is actually a series of commands for tools like editors and `patch`
telling them how to reconstruct one file given the other.
If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which revisions of the file
    Git is comparing;
    `df0654a` and `315bf3a` are unique computer-generated labels for those revisions.
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` markers in the first column show where we are adding lines.

After reviewing our change, it's time to commit it:

~~~ 
$ git commit -m "Added what type of a script we are developing"
~~~

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that:

~~~ 
$ git add README.md
$ git commit -m "Added what type of a script we are developing"
~~~

## Challlenge!

Add the following text to the file and add and commit the changes!

`The script will function to extract the unique values from a column from a .csv file saves these to a file.`

## Setting up a repository on Github

Get students to put their repository on Github and then show them History and click on commits to show what changed between each version of the file. This is why Git with Github is amazing!!

## Let's make another change and push it to Github

Add the following text to the `README.md` file and add, commit and push the file.

~~~
## List of Dependencies
~~~

## Ignoring Things

We are going to develop a script, so we need some data to test it on. Let's use the antibiotics.csv file in nelle's directory. Let's make a data directory in our project's directory and move the file there. 
~~~
mkdir data
mv ../../Shell/Users/nelle/antibiotics.csv data/antibiotics.csv
ls data
~~~

~~~
git status
~~~

Git tells us there's an untracked file... Question - do we want to track it? As a general rule, no. We don't usually version control data. So, let's tell git to ignore all the files we put in the data directory.

We do this by creating a file in the root directory of our project called `.gitignore`.

~~~ 
$ nano .gitignore
$ cat .gitignore
~~~
~~~
data/
~~~

These patterns tell Git to ignore the data directory and everything in it. Note - you can also add specific files to this list if want. 

Once we have created this file,
the output of `git status` is much cleaner:

~~~ 
$ git status
~~~

The only thing Git notices now is the newly-created `.gitignore` file.
You might think we wouldn't want to track it,
but everyone we're sharing our repository with will probably want to ignore
the same things that we're ignoring.
Let's add and commit `.gitignore`:

~~~ 
$ git add .gitignore
$ git commit -m "Add the ignore file"
$ git status
~~~
~~~ 
git push origin master
~~~

## Challenge!
Add a list of the programs our script will depend on and add, commit and push these changes. Look at your Github repo - can you see the changes there?

## Recovering old versions
Let's open README.md again and make another change:

~~~
nano README.md
cat README.md
~~~

~~~
# Extract Unique Elements
Tiffany Timbers, October 6, 2015

This project is to develop a Bash Shell script.
## List of Dependencies
~~~

Oh no! We made a mistake! We didn't mean to delete our list of dependencies. Worry not! We can now take advantage that we have been tracking this file under version control by checking out an older version of the file to replace the broken version we now have.


We can look at the history on Github to see which version of the file we want to go back to. This depends on whether we have pushed everything to Github, so we can also see our history locally via Gitlog

~~~
git log
~~~

We can `git diff` on the command line to compare the files to make sure this is the one we want to go back to:

~~~
git diff f22b25e README.md
~~~

Then use `git checkout` in the command line to grab it.

~~~
git checkout f22b25e README.md
cat README.md
~~~

~~~
# Extract Unique Elements
Tiffany Timbers, October 6, 2015

This project is to develop a Bash Shell script.
## List of Dependencies
1. Bash Shell
2. .csv file delimited by commas
~~~

Yeah! We recovered our file, let's check git status!

~~~ 
git status
~~~








