# Welcome to the Terminal!  

This will be a short introduction to some basic concepts on using the terminal and Bash. For the purposes of this tutorial, it will be taught from the perspective of those using the `bstreutlein` machines. This also assumes the user is using a Mac (relevant to how to log in)



## Logging in

The `bstreutlein` clusters can be accessed by `SSH` (Secure Shell).  

Ex.: `ssh username@d@bs-treutlein02`  

SSH has many options (check them by `ssh --help` or `ssh -h` and Googling). We may also be interested in establishing _ssh tunnels_. These tunnels listen to a specific port, as channel it into another.

Ex.: `ssh username@d@bs-treutlein02 -L 8788:localhost:8788`  

This will listen to port 8788, and its communications can be accessed at the localhost (i.e. your computer) on port 8788. Incidentally, port 8788 transmits the RStudio Web application which has other ways of being accessed, but other uses for this include setting up your own Jupyter Labs session.  


## Computing structure

When you log in, you will land on your `home` folder. These have a limited space, so they should be used for nothing other than they already contain. Our server also has access to the `/links` and `/local1`/`/local2` file systems. These are essentially "disks" where data is stored. While they have no CPU/RAM (those are in the server themselves), they can still impact cluster processing/efficiency. The `/links` storage is the largest (essentially limitless) but is very slow in reading and writing files. For large data acess, the `/local` file systems should be used.  

To check computational resource usage, you can use `htop`. This will show you all computing threads, as well as RAM usage and the individual processes consuming these resources below.


## Moving and exploring folders

You can find out in what directory you are by typing `pwd` (present working directory). Some terminals will also display this information next to the entry point.  

You can move between directories by using `cd`. Here are some useful ones:  

 - `cd /links/groups/treutlein` - go to the group folder
 - `cd /local1/USERS` - go to the `/local1` file system
 - `cd ~` - the same as `cd /home/username`
 - `cd ..` - move to the parent directory
 - `cd -` - go to the previous directory you were in
 
Importantly, you can also use the "TAB" key to autocomplete paths and commands. For example, if you type `cd /lin` and press TAB, it should autocomplete to `cd /links`, or suggest alternatives starting with lin.  

You can also create directories, e.g. `mkdir test`; and delete them, e.g. `rm -fr test`. `rm` is a general remove function that can also work in files. The `-fr` options mean "force" (mandatory for directories to confirm our intention) and "recursive" (remove this directory and all subdirectories and files it contains).

But what's in a folder? You can find that out by using the `ls` command. This lists all files and folders in your current directory (or any other that you pass as an argument), and some systems will colour these differently. you can also use `ls -lh` for a full list of details with human-readable file sizes, or `ls -a` to reveal hidden files/folders - these always start with a ".".
 
 
## Your PATH

Where do these commands come from? You can find this by typing, for example, `which mkdir`. You will see that this programme is installed in `/usr/bin/`. But how does it know where to look?  

Folders are searched for the commands you enter if they are added to the `PATH` variable. You can see your own path by typing `echo $PATH` (the dollar sign denotes we want a variable, not just the text "PATH"). Other system variables exist that can be defined or called by specific applications. While a few directories are already added by default to your PATH, you can yourself add more directories to it.  

Ideally, these would be defined in such a way that once you choose it, they are loaded every time you start your session. For this, you can count on `.bashrc`, a hidden file in your home directory that is executed every time you connect to the server. The PATH variable can be edited as follows:

 1. Type `nano ~/.bashrc` (`nano` is a text editor for the console. Other editors include `vi` or `emacs`) 
 2. In a new line, add `export PATH=/path/to/dir:$PATH`. This defines `$PATH` as that new directory, and all the others already present in the PATH. Notice the separating ":".
 3. Exit by following the instructions at the bottom of the screen.

Other things can be added to the `.bashrc` file. For example, if there is a directory with a long path that you frequently change to (e.g. your user folder in `/local1`), you can add this line to `.bashrc`:

`alias golocal='cd /local1/USERS/username'`  

And now every time you type `golocal`, the other command is executed.


## Working with files

You can create a file with `nano newfile.txt`. The file extension is not really necessary, but it helps us distinguish what type of file we're talking about (in this case, a text file). Copy the following into this file:  

Testing  
TESTING  
1,  
2,  
3,  
4,  
5,  
Testing  

And save. It's a great file. So we can make more of it with the `cp` command:  

`cp newfile.txt newfile2.txt`  

You can also point it to another directory and make the copy there. And, if you want to copy a whole directory into another (with all contents), you can use `cp -R /some/dir /target/dir`.  

newfile2.txt isn't that nice of a name. We can rename the file by doing `mv newfile2.txt greatfile.txt`. Incidentaly, the `mv` command can also be used to move files, so `mv greatfile.txt /local1/greatfile.txt` would delete it from your current directory and put it in `/local1`.

You can check the top/bottom of your file with `head`/`tail`. By default 10 lines are shown, but you can change this with the `-n` parameter (e.g. `head -n 2 greatfile.txt`). You can also incrementally print the file with `more greatfile.txt`, where first the console will be filled with all possible lines, and the you can show new ones by pressing Enter (+1 line) or Space (+1 full screen).  

A file can also be completely printed by running `cat greatfile.txt` - but don't do this with big files! Or, if you do, you can stop the printing (or indeed any process for that matter) with Ctrl+C. You can also kill processes running in the background with `kill -9 processID`, but be very sure you want to do this!  

Testing is nice, but Resting is better. We can replace these words in our file with `sed`. See the examples below:

 - `sed 's/Testing/Resting/g' greatfile.txt` - will print the full file with all occurences of "Testing" as "Resting"
 - `sed -i 's/Testing/Resting/g' greatfile.txt` - will change the file to have all occurences of "Testing" as "Resting"
 - `sed 's/Testing/Resting/gI' greatfile.txt` - will print the full file with all occurences of "Testing" as "Resting", without regard for capitalization.  
 
For more details on sed, it's best to Google for specific uses.  

These programmes print what you want to see because, by default, they _write to standart ouput_ (i.e. the terminal). You can change this by pointing to which file they should write:  

 - `cat newfile.txt greatfile.txt > otherfile.txt` - will concatenate those two files _into a new file_. If that file already exists, IT WILL BE OVERWRITTEN.
 - `cat newfile.txt greatfile.txt >> otherfile.txt` - will concatenate those two files and _append them_ to an existing file.  
 
Try checking this `otherfile.txt` with `cat` - it's huge!... but how long? You can use `wc -l otherfile.txt` to count the number of lines there. Or, simply, `wc otherfile.txt` will print the number of lines, words, and bytes that a file has.  

And how many times does the word "Testing" appear? For that, we can use grep:  

`grep -c "Testing" otherfile.txt`  

You can also remove the -c (`grep "Testing" otherfile.txt`) to just print all lines where the word occurs, though in this case, as we saw, each line only has one word.  

And if you forget about the -c, you may at least remember that `wc -l` will count the number of lines. So you can _pipe_ the result of one function into another, as such:  

`grep "Testing" otherfile.txt | wc -l`  

But maybe you really want to remember the command! For this you can access your `history` (all the commands that you've ran), and use `grep` to find it:

`history | grep grep | tail`  

Lastly, this file is big, so we should compress it so save space. You can do that with `gzip otherfile.txt`, and `gzip -d otherfile.txt` to decompress.


# Downloading files

Now that you zipped your otherfile, you can bring it to your own laptop. To do this, you can use `rsync`. For example, in your local terminal (not the one connected to the cluster), you can run:

`rsync username@d@bs-treutlein02:/home/username/otherfile.txt ./`

After inserting the password, this will bring that file to your local machine.  

You can also obtain publicly available files by using `wget`, e.g. `wget http://jaspar2018.genereg.net/download/CORE/JASPAR2018_CORE_vertebrates_non-redundant_pfms_jaspar.txt`. You can also use `curl` for this (installed on Mac).


## Screens

Sometimes programmes will run for a long time in the console. To have them running in the background, but still interactively, you can use `screen -DR`. This will start a parallel session, where you can have as many consoles as you want (without having to log in multiple times). You can create new windows with Ctlr+A,Ctrl+C, and cycle through them with Ctrl+A,Ctrl+Space. You can quit your screen back to the main terminal with Ctrl+A,Ctrl+D.

Lastly, before we leave, we should cleanup the directory you were working on (likely the home directory). First check all the files you have in it. If (and only if!) all the files ending in ".txt" were those created by us, you can use `rm *.txt` to remove all files whose names end in ".txt".


