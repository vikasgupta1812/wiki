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
	

### 
