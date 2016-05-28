Linux commands May 22 

What should I do when Ubuntu freezes?

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

----

	