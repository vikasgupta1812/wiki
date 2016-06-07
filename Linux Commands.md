## Linux commands

### What should I do when Ubuntu freezes?

You can `REISUB` it, which is a safer alternative to just cold rebooting the computer.  REISUB by:

While holding `Alt` and the `SysReq` (`Print Screen`) keys, type `R``E``I``S``U``B`

	R:  Switch to XLATE mode
	E:  Send Terminate signal to all processes except for init
	I:  Send Kill signal to all processes except for init
	S:  Sync all mounted file-systems
	U:  Remount file-systems as read-only
	B:  Reboot
	
REISUB is BUSIER backwards, as in "The System is busier than it should be", if you need to remember it. Or mnemonically - R eboot; E ven; I f; S ystem; U tterly; B roken.


NOTE: There exists less radical way than rebooting the whole system. If `SysReq` key works, you can kill processes one-by-one using `Alt`+`SysReq`+`F`. Kernel will kill the mostly «expensive» process each time. If you want to kill all processes for one console, you can issue `Alt`+`SysReq`+`K`.

[source](http://askubuntu.com/questions/4408/what-should-i-do-when-ubuntu-freezes)

### Command: lsblk
The “lsblk” stands for (List Block Devices), print block devices by their assigned name (but not RAM) on the standard output in a tree-like fashion.

	# lsblk

	NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
	sda      8:0    0 232.9G  0 disk 
	├─sda1   8:1    0  46.6G  0 part /
	├─sda2   8:2    0     1K  0 part 
	├─sda5   8:5    0   190M  0 part /boot
	├─sda6   8:6    0   3.7G  0 part [SWAP]
	├─sda7   8:7    0  93.1G  0 part /data
	└─sda8   8:8    0  89.2G  0 part /personal
	sr0     11:0    1  1024M  0 rom
The “lsblk -l” command list block devices in ‘list‘ structure (not tree like fashion).

	root@tecmint:~# lsblk -l

	NAME MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
	sda    8:0    0 232.9G  0 disk 
	sda1   8:1    0  46.6G  0 part /
	sda2   8:2    0     1K  0 part 
	sda5   8:5    0   190M  0 part /boot
	sda6   8:6    0   3.7G  0 part [SWAP]
	sda7   8:7    0  93.1G  0 part /data
	sda8   8:8    0  89.2G  0 part /personal
	sr0   11:0    1  1024M  0 rom
Note: `lsblk` is very useful and easiest way to know the name of New Usb Device you just plugged in, especially when you have to deal with disk/blocks in terminal.


### 3. Command: md5sum
The “md5sum” stands for (Compute and Check MD5 Message Digest), md5 checksum (commonly called hash) is used to match or verify integrity of files that may have changed as a result of a faulty file transfer, a disk error or non-malicious interference.

	root@tecmint:~# md5sum teamviewer_linux.deb 

	47790ed345a7b7970fc1f2ac50c97002  teamviewer_linux.deb
Note: The user can match the generated md5sum with the one provided officially. 


### Command: dd
Command “dd” stands for (Convert and Copy a file), Can be used to convert and copy a file and most of the times is used to copy a iso file (or any other file) to a usb device (or any other location), thus can be used to make a ‘Bootlable‘ Usb Stick.

	# dd if=/home/user/Downloads/debian.iso of=/dev/sdb1 bs=512M; sync
Note: In the above example the usb device is supposed to be `sdb1` (You should Verify it using command lsblk, otherwise you will overwrite your disk and OS), use name of disk very Cautiously!!!.

### Command: uname
The “uname” command stands for (Unix Name), print detailed information about the machine name, Operating System and Kernel.

	# uname -a

	Linux tecmint 3.8.0-19-generic #30-Ubuntu SMP Wed May 1 16:36:13 UTC 2013 i686 i686 i686 GNU/Linux
Note: uname shows type of kernel. `uname -a` output detailed information. Elaborating the above output of `uname -a`.

	“Linux“: The machine’s kernel name.
	“tecmint“: The machine’s node name.
	“3.8.0-19-generic“: The kernel release.
	“#30-Ubuntu SMP“: The kernel version.
	“i686“: The architecture of the processor.
	“GNU/Linux“: The operating system name.
	

### cal
The “cal” (Calendar), it is used to displays calendar of the present month or any other month of any year that is advancing or passed.

	root@tecmint:~# cal 

	May 2013        
	Su Mo Tu We Th Fr Sa  
	          1  2  3  4  
	 5  6  7  8  9 10 11  
	12 13 14 15 16 17 18  
	19 20 21 22 23 24 25  
	26 27 28 29 30 31
Show calendar of year 1835 for month February, that already has passed.

	root@tecmint:~# cal 02 1835

	   February 1835      
	Su Mo Tu We Th Fr Sa  
	 1  2  3  4  5  6  7  
	 8  9 10 11 12 13 14  
	15 16 17 18 19 20 21  
	22 23 24 25 26 27 28
Shows calendar of year 2145 for the month of July, that will advancing

	root@tecmint:~# cal 07 2145

	     July 2145        
	Su Mo Tu We Th Fr Sa  
	             1  2  3  
	 4  5  6  7  8  9 10  
	11 12 13 14 15 16 17  
	18 19 20 21 22 23 24  
	25 26 27 28 29 30 31
	

### Configure SSH config for less typing

Configure `~/.ssh/config` so you can just type a shortcut (e.g. ssh aws) to connect.

	mkdir -p ~/.ssh
	mv ~/Downloads/identity.pem ~/.ssh
	chmod 400 ~/.ssh/identity.pem # if you haven't already
	chmod 700 ~/.ssh
	nano ~/.ssh/config # see below for content
	chmod 600 ~/.ssh/config

Contents of `~/.ssh/config` should look like this.

	Host aws
	Hostname ec2-54-218-72-128.us-west-2.compute.amazonaws.com # or use the IP address
	User ubuntu
	IdentityFile "~/.ssh/identity.pem"


### Transferring files

Copy files to/from the server using `scp` (GUI options exist and one is built-in to Ubuntu). E.g. to copy a file from the server to your local machine:

	scp -i identity_file.pem ubuntu@ec2-01-234-56-789.compute-1.amazonaws.com:~/foo/bar.txt .

Or to copy from local machine to AWS instance (and place it in $HOME):

	scp -i identity_file.pem file1 file2 ubuntu@ec2-01-234-56-789.compute-1.amazonaws.com:~/

If you set up your SSH config file , then scp becomes a lot simpler to use:

	scp foo.txt aws:~/bar.txt
	

### (Re)claim ownership of directories

Some commands, like `npm`, require superuser permissions. Instead of using `sudo` every time, you can claim ownership of the appropriate directory. E.g. `sudo chown -Rwhoami~/.npm`

- `rsync` is generally a better alternative to scp
- `ln -s` for creating symbolic links (having a file in two places at once via a pointer)
- cut commonly used for selecting fields (default is to delimit by tab)
- sort for sorting, has many options
- uniq for unique data
- wc for many types of counts, e.g. words, chars, lines, longest line (for pre-allocating a buffer)
- split splits files, handy for parallel processing jobs
- paste combines columns (commonly used with cut)
- wget & curl for grabbing online resources; curl is handy for APIs
- nl prepend line numbers
- xargs execute commands from STDIN, useful when dealing with a huge number of files (use -P to enable multiple processors)
- find non-indexed search, e.g. find /etc | nl
- locate indexed search. works out of the box on Ubuntu, but on MacOS you need to do
	sudo apt-get install -y locate;
	sudo updatedb;
	locate fstab
- grep print lines matching a pattern
- sed stream editor for filtering and transforming text, useful for search and replace - [one liners](http://www.catonmat.net/blog/wp-content/uploads/2008/09/sed1line.txt)
- awk pattern scanning and text processing language, useful for tab-delimited data - [one liners](http://www.pement.org/awk/awk1line.txt)

#### [Bash keyboard shortcuts](http://www.howtogeek.com/howto/ubuntu/keyboard-shortcuts-for-bash-command-shell-for-ubuntu-debian-suse-redhat-linux-etc/)
Should also work in fish shell.

- <kbd>CTRL</kbd> + <kbd>l</kbd>: clear screen
- <kbd>CTRL</kbd> + <kbd>u</kbd>: clear text before cursor
- <kbd>CTRL</kbd> + <kbd>c</kbd>: kill current process
- <kbd>CTRL</kbd> + <kbd>d</kbd>: exit current shell
- <kbd>CTRL</kbd> + <kbd>z</kbd>: puts the current process in the background, restore with `fg`  
  You can also use <kbd>&</kbd>. E.g. `sleep 10 &`
- <kbd>CTRL</kbd> + <kbd>w</kbd>: clear word before cursor
- <kbd>TAB</kbd>: auto-complete



#### Install Python numpy, scipy, pandas, and sklearn
```
$ sudo apt-get install python-numpy
$ sudo apt-get install python-scipy
$ sudo apt-get install python-pandas
$ sudo apt-get install python-sklearn
```



#### `screen` tab manager for remote sessions

Allows you to reconnect and resume your session where you left off. Useful if a remote connection drops or for any long-running computation (running as a background process--see above--is a viable alternative).

`tmux` is considered [a better alternative](http://honnef.co/posts/2010/10/why_you_should_try_tmux_instead_of_screen/) to `screen`. Use it with [teamocil](https://github.com/remiprev/teamocil) to automatically create sessions, windows and panes with YAML files.

In `screen`, type
- `C-t` `?` to bring up the help
- `C-t` `c` to create new tabs
- `C-t` `u` or j to switch tabs
- `C-t` `0..9` to go to a specific tab (starts at 0)
- `C-t` `C-t` to go back and forth between the current and previous tab
- `C-d` to exit the current tab
- `C-t` `[` to go into copy mode  
  You can use the arrow keys to move around or `C-p`/`n` to move up and down  
  copy stuff with `(space)` or `(enter)`, you have to set beginning and end  
  paste with 'C-t ]'  
  abort with 'C-(space)'

Note: <kbd>C-t ?</kbd> means <kbd>CTRL</kbd> and <kbd>t</kbd> followed by <kbd>?</kbd>.


#### How to install specific git commit with pip 

```
You can specify commit hash, branch name, tag.

$ pip install git+git://github.com/aladagemre/django-notification.git@2927346f4c513a217ac8ad076e494dd1adbf70e1

$ pip install git+git://github.com/aladagemre/django-notification.git@cool-feature-branch

$ pip install git+git://github.com/aladagemre/django-notification.git@v2.1.0
```


- type `echo $SHELL` to see your shell
- who displays a list of users that are currently logged in
- who am i (whoami) tells you the current user name

```
$cal dec 2016
   December 2016
Su Mo Tu We Th Fr Sa
             1  2  3
 4  5  6  7  8  9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28 29 30 31
```
```
$cal july 2015
     July 2015
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30 31
```
Week number

```
$ncal -w july 2015
    July 2015
Mo     6 13 20 27
Tu     7 14 21 28
We  1  8 15 22 29
Th  2  9 16 23 30
Fr  3 10 17 24 31
Sa  4 11 18 25
Su  5 12 19 26
   27 28 29 30 31
```



Control Sequences

```
Ctrl + l clear screen
Ctrl + c stop current command
Ctrl + z suspend current command
Ctrl + k kill to end of line
Ctrl + r search history
Ctrl + n next history item
Ctrl + p previous history item
```

- `ls -1` (one entry per line)
- The most commonly used file viewer is `less`
