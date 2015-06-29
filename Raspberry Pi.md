
- Install ELK 
	Raspberry Pi ELK Stack - https://gist.github.com/vjm/d206171be8971294f98b
	An example SupervisorD configuration for all three logstash components. - https://gist.github.com/vmadman/5472139

- Know your Raspberry Pi - http://raspberry-pi-guide.readthedocs.org/en/latest/system.html


- Mounting USB Drive 
	https://www.raspberrypi.org/forums/viewtopic.php?t=38429

	```
	/dev/sdb1: LABEL="UNTITLED" UUID="78A4-1DEF" TYPE="vfat" 


	/dev/sda1       /media/HDD1      ntfs-3g defaults,noatime        0       0
	/dev/sdb1       /media/usbdrive      ect4 defaults,noatime        0       0



	sudo mount /dev/sdb1 /media/usbdrive
	sudo chown pi:pi /media/usbdrive
	sudo chmod 777 /media/usbdrive
	```




- Install MySQL - http://www.raspipress.com/2014/06/tutorial-install-mysql-server-on-raspbian/
