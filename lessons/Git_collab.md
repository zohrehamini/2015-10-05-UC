## Practicing in pairs 

Designate one partner as the repository "Owner" and one partner as the repository "Collaborator". 

The repository "Owner" needs to grant the Collaborator access.

#####Owner: 
* On GitHub, click the settings button on the right. 
* Select Collaborators (top left), and enter your Collaborator's username.

#####Collaborator: 
* Go to your email to retrieve the `URL` to connect to the Owner's repository.
* `cd` to another directory
(so `ls` doesn't show a `unique_extract` folder),
and then make a copy of this repository on your own computer:

~~~ 
$ git clone URL_from_Collaborator
~~~

`git clone` creates a fresh local copy of a remote repository.

* Make a change in their copy of the repository:

~~~ {.bash}
$ cd unique_extract
$ nano README.md
$ cat README.md
~~~

~~~ 
Example of how to use the script below:
~~~

~~~
$ git add README.md
$ git commit -m "Added that we will put a example call of the script in the README."
$ git push origin master
~~~


Now the Owner needs to update their repository to access the changes made by the Collaborator.

#####Owner: 
* retrieve the changes from Github:

~~~ 
$ git pull origin master
~~~

Now the repositories on both the Owner's and the Collaborator's machines should be
up to date and identical. We can check that this is the case for the `README.md` file.

**Owner & Collaborator**: check the contents of `README.md`

~~~
cat README.md
~~~

## GitHub Collaboration Challenge!

* Now the repository owner should create a file called `extract.sh` that contains a comment with their name & date
* add and commit the changes to Git and push the changes to Github. 
* Then the collaborator should pull the changes 
from the remote repository to their local repository.
* Finally, use `cat` to verify
that both the Owner and Collaborator have the same version of the file.


* Now have the collaborator add a comment to the script about what it is supposed to do, add, commit and push the change to Github and have the owner pull the change.


## Hint

Before teaching this next section, the collaborator should make a secret change to the `extract.sh` file and add, commit and push the changes to the remote repository.

Collaborators change should be:

~~~
cut -f $1 -d ',' data/antibiotics.csv | sort | uniq > antibiotics_unique.csv
~~~


As soon as people can work in parallel,
someone's going to step on someone else's toes.
This will even happen with a single person:
if we are working on a piece of software on both our laptop and a server in the lab,
we could make different changes to each copy.
Version control helps us manage these [conflicts](reference.html#conflicts)
by giving us tools to [resolve](reference.html#resolve) overlapping changes.

#####Owner: 
* Make a change to the `extract.sh` file.

~~~ 
$ nano extract.sh
$ cat extract.sh
~~~
~~~ 
# Tiffany Timbers, October 6, 2015
cut -f 2 -d ',' data/antibiotics.csv | sort | uniq > antibiotics_unique.csv
~~~

* Let's test that it works!
~~~
bash extract.sh
ls
~~~

* Whoohoo, it works! Let's commit the change locally:

~~~ 
$ git add extract.sh
$ git commit -m "First draft of the script"
~~~

* Try to push the change to Github:

~~~ 
$ git push origin master
~~~

but Git won't let us push it to GitHub!

Git detects that the changes made in one copy overlap with those made in the other
and stops us from trampling on our previous work.
What we have to do is pull the changes from GitHub,
[merge](reference.html#merge) them into the copy we're currently working in,
and then push that.

* Start by pulling:

~~~ 
$ git pull origin master
~~~

`git pull` tells us there's a conflict,
and marks that conflict in the affected file:

Owner: Look at contents of `extract.sh`

~~~ 
$ cat extract.sh
~~~

It is now up to us to edit this file to remove these markers
and reconcile the changes.
We can do anything we want: keep the change made in the local repository, keep
the change made in the remote repository, write something new to replace both,
or get rid of the change entirely.

Owner: Use a text editor to resolve the conflict:

~~~ 
$ nano extract.sh
$ cat extract.sh
~~~
~~~ 
# Tiffany Timbers, October 6, 2015
cut -f $1 -d ',' data/antibiotics.csv | sort | uniq > antibiotics_unique.csv
~~~

Let's test that the script works with Tim's changes:

~~~
bash extract.sh 2
~~~

Owner: To finish merging, we add `extract.sh` to the changes being made by the merge
and then commit:

~~~ 
$ git add extract.sh
$ git commit -m "Merged conflict on first draft of the script - Tim, I really like your suggestion of using a variable so we can change the column we operate on!"
$ git push origin master
~~~

Git keeps track of what we've merged with what,
so we don't have to fix things by hand again.

#####Collaborator: 
* Pull the changes from Github:

~~~
$ git pull origin master
~~~
We get the merged file:

* Look at contents of `extract.sh` to verify it is now the same as the 
Owner's copy:

~~~ 
$ cat extract.sh
~~~

We don't need to merge again because Git knows someone has already done that.

Version control's ability to merge conflicting changes
is another reason users tend to divide their programs and papers into multiple files
instead of storing everything in one large file.
There's another benefit too:
whenever there are repeated conflicts in a particular file,
the version control system is essentially trying to tell its users
that they ought to clarify who's responsible for what,
or find a way to divide the work up differently.

## GitHub Conflict Challenge!
* Now the Owner should make a change to `extract.sh` to allow the user to specify the filename so we can use this with different files. Add, commit and push the changes to Github. 
* Next the Collaborator should also modify `extract.sh`, but make a change that allows the user to specify the name of the file we save to. Now add, commit and try to push the file to Github. If there's a conflict, resolve it.