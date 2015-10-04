# The Unix Shell
2015-10-05 UC SWC workshop,
Instructor: Tiffany Timbers

## Introduce the Shell
#### Key concepts to cover
* Explain how the shell relates to the keyboard, the screen, the operating system, and 
users' programs.
* Explain when and why command-line interfaces should be used instead of graphical 
interfaces
* Introduce Nelle Nemo

## The Filesystem
note - have the students download `data.zip` and copy unzipped `data` directory to their 
`Desktop`
* Open the shell
* Explain the prompt: `$`
* Example command: `pwd`
* Introduce absolute path and `root` directory
* Introduce `ls` and `ls -F`
* Introduce how to get documentation (e.g., `man ls` or `ls --help`) or google!
* Introduce `cd` via `cd Desktop`, and discuss relative path
* Get students to navigate to the `~/Desktop/data/Shell/Users/nelle` directory
* Do `pwd` again so students can see where they are
* Do ls to see what is in the directory
* Look in `data` directory via: `ls -F data` and explain command, argument and directory/
filename in CLI call
* Move into the `data` directory via `cd data`
* Show where we moved to: `pwd` and what is in that directory: ls -F
* How do we move back up to `nelle`? Two ways - discuss absolute and relative path again.
* What is `..`? 
* Show `ls -a`
* We see `.` (current directory), `..` (parent directory), and `.bash_profile` (file 
usually containing settings to customize the shell)
* Introduce tab completion via navigating into `cd north-pacific-gyre` and `cd 2012-07-03`

#### Multiple Choice Questions 1 - 3 via Socrative.org (3 questions)

## Creating Things
* Navigate back to `nelle` directory
* Introduce `mkdir` via `mkdir thesis`
* Use `ls -F` to see what changed
* Look to see if there is anything inside the `thesis` directory via `ls thesis -F`
* Move into the `thesis` directory 
* Create a new document there called `draft.txt` via `nano draft.txt`
* Introduce nano, explain why we use it for the workshop, and what they might want to use
outside of the workshop.
* In nano, write: `It's not "publish or perish" anymore, it's "share and thrive".` and 
save and exit out of nano
* Check to see what is in the `thesis` directory now: `ls`

## Removing Things
* Let's delete what we just wrote via `rm draft.txt`
* See what has changed via `ls`
* Explain how deleting is FOREVER in the Shell
* Recreate a file called `draft.txt` in the `thesis` directory containing: `It's a start`
* Let's try to delete whole `thesis` directory, first need to navigate up via `cd ...` and 
the try `rm thesis`
* We get an error because `thesis` is a directory
* To remove a directory we need to use `rmdir thesis`
* This doesn't work either because there are files in the directory. We have two options,
individually delete the files inside the directory and then delete the directory or use a 
command which recursively deletes files: `rm -r thesis`. Use `rm -r` with CAUTION!
* Delete thesis directory via option 1: `rm thesis/draft.txt` and then `rmdir thesis`

## Moving Things
* Let's create the `thesis` directory one last time: `mkdir thesis`
* And create a file called `draft.txt` containing `"some short funny quote about thesese or persistance"`
* Let's rename this file and give it a much more informative name, e.g., `quotes.txt`
* We rename files via the `mv` command: `mv thesis/draft.txt thesis/quotes.txt`
* Check that this worked: `ls thesis`
* Note `mv` also works on directories
* We can also use the `mv` command to actually move things, for example moving `quotes.txt`
to the `nelle` directory: `mv thesis/quotes.txt .`
* Check that this worked: `ls . thesis`

## Copying Things
* We can also copy things via the `cp` command
* Let's put a copy of `quotes.txt` back in the thesis directory: `cp quotes thesis/quotes.txt`
* Check that this worked via `ls . thesis` 

#### Multiple Choice Questions 4 - 6 via Socrative.org (3 questions)

## Pipes and Filters
Now that we know a few basic commands, we can finally look at the shell's most powerful 
feature: the ease with which it lets us combine existing programs in new ways. 

We'll start with a directory called molecules that contains six files describing some 
simple organic molecules. The .pdb extension indicates that these files are in Protein 
Data Bank format, a simple text format that specifies the type and position of each atom 
in the molecule.

* Take a look at what is in the `molecules` directory: `ls molecules`
* Let's look in one of these files, we can use the `cat` command to do this: 
`cat octane.pdb`
* What if we want to quickly look at the number of lines, characters or words in these 
files? We could do that using the `wc` command: `wc octane.pdb`
* How can get this information for all of these files?
* We can use `*` to do this! `wc *.pdb`
* If we are only interested in the number of lines in each file we can use the `-l` flag:
`wc -l *.pdb`
* Which of these files is the shortest? It's pretty obvious with only 6 files, but what if 
we have 6000 files?
* We could solve this problem as so:
`wc -l *.pdb > lengths.txt`
`cat lengths.txt`
`sort -n lengths.txt > sorted_lengths.txt`
`cat sorted_lengths.txt`
`head -1 sorted_lengths.txt`
* Easy and clear, right? Not really! We can do better by using pipes!
`wc -l *.pdb | sort -n`
* We can even add the third command on with another pipe!
`wc -l *.pdb | sort -n | head -1`
  
#### Multiple Choice Questions 7 & 8 via Socrative.org (2 questions)

## Loops
We have learned a number of ways to save time (e.g. tab completion, wild-cards and pipes),
now we are going to learn another way to save time, that works especially well when you 
need to do the same task over and over again.
* Go to loops figure of `animals` and `creatures` directories
* Get students to move to `nelle/creatures`
* Inside this directory we have 2 files: `ls`
* We are going to work towards a loop that renames these files, but let's start with a 
simpler case, let's make a loop that prints the first 3 lines of these files:
`$ for filename in basilisk.dat unicorn.dat`
`> do`
`> head -3 $filename`
`> done`
* It works! Now what if we had 600 files? We don't want to type each out individually...
We can use a wild-card to make this easier!
`$ for filename in *.dat`
`> do`
`> head -3 $filename`
`> done`
* OK, now that we understand loops, let's go back to our original problem and write a loop
to rename these files:
`$ for filename in *.dat`
`> do`
`> mv $filename original-$filename`
`> done`

#### Multiple Choice Questions 9 & 10 via Socrative.org (2 questions)

* Loops are fantastic and they save us a lot of time, but when we make mistakes, we can 
make a lot of them with a loop. 
* One way to avoid this is to use the echo command to print the commands to the screen 
instead of actually running them. If that looks good then we can go ahead and remove the 
command and run the loop.
`$ for filename in *.dat`
`> do`
`> echo head -3 $filename`
`> done`

## Shell Scripts
We are now going to learn how to take the commands that we repeat frequently and save them 
in a file so that we can re-run all of the operations again later by typing a single 
command. We will essentially writing small programs.
* Direct students to `molecules` directory
* Create a file called `middle.sh` containing: `head -15 octane.pdb | tail -5` which will
print lines 11-15 of the file `octane.pdb`
* Now we can simply run this set of commands by simply typing: `bash middle.sh` to execute
the commands within the file.
* This program isn't very flexible and can currently only be used on `octane.pdb`
* We can make it more flexible by using variables and taking in command line arguments. In 
the script we replace `octane.pdb` with `$1` (a special variable): `head -15 $1| tail -5`
* Now when we call `middle.sh` we can now specify which file we want to apply this to. 
E.g., `bash middle.sh cubane.pdb`
* What if we want to make it even more flexible? Say, for example which lines we want to 
grab? We can do this by using more variables: `head -$2 $1 | tail -$3`
* So lets grab lines 3-5 of a file: `bash middle.sh cubane.pdb 5 3
* Now, we know what this script does, but will we remember in 6 months from now? I wont!
* We can use comments to document what the code does for our future selves and for anyone
who we share it with: 
`# Selects lines from the middle of a file`
`# Usage: middle.sh filename end_line number_of_lines`
`head -$2 $1 | tail -$3`

#### Multiple Choice Questions 11 & 12 via Socrative.org (2 questions)

## History
* In practice, many people develop shell scripts by running commands at the prompt a few 
times to make sure they are doing the right thing, and then they save those commands to a
file (i.e., a script) for re-use. 
* To capture your Bash Shell history you type `history`
* That prints everything of the screen, so we can use tail to limit that: `history | tail -10`
* You can send the output to a file as well `history | tail -10 > myscript.sh`
* `cat myscript.sh`
* This almost looks like a script, but there are weird numbers at the beginning...
* We can remove this with the `colrm` command: `history | tail -5 | colrm 1 7 > myscript.sh`
* `cat myscript.sh`

## Finding Things

* `grep`
#### Multiple Choice Question 13 via Socrative.org (1 question)

* `find`
#### Multiple Choice Question 14 via Socrative.org (1 question)
