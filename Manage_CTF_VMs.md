# How to Manage CTF VMs

Hypervisors:
ssh hobbes.sus.mcgill.ca
ssh calvin.sus.mcgill.ca


GUI: 
https://hobbes.sus.mcgill.ca:8006

To create an account to access the GUI, ask the server admin to create an account on hobbes.sus.mcgill.ca by:
	1. ssh hobbes.sus.mcgill.ca
	2. `sudo adduser whatever-user`
	3. On GUI, go to the **Users** tab
	4. **Add** user, and remember to specify the first and last name
	5. Go to the **Permissions** tab. Give the new user **User Permission**.
	6. Path should be /, checkmark propagate, and give the minimum Role the user needs.
User should be able to log in to GUI at this point.

How to Create VM 
1. Download Debian netinstall iso (newest version)
	* Choose amd64 
	* https://www.debian.org/CD/netinst/ 
 	![Alt](/1.png "")
2. Go to the GUI, click **local** storage -> **Content** tab -> **Upload** if the latest version is not already there.
	![Alt](/2.png "")
	![Alt](/3.png "")
	![Alt](/4.png "")
	![Alt](/5.png "")
3. Click **Create VM**
	![Alt](/5_1.png "")
4. General: Set VM ID and name -> Next
	![Alt](/5_2.png "")
5. OS: Choose **Linux 3.X/2.6 Kernel (l26)** -> Next
	![Alt](/5_3.png "")
6. CD/DVD: Use CD/DVD disc image file (iso) with local storage and the latest Debian ISO
	![Alt](/5_4.png "")
7. Hard Disk: Local storage -> change disk size as needed -> QEMU format -> Next
	![Alt](/5_5.png "")
8. Change CPU setting as needed -> Next
	![Alt](/5_6.png "")
9. Change Memory size as needed -> Next
	![Alt](/5_7.png "")
10. Leave Network as is -> Next
	![Alt](/5_8.png "")
11. Confirm: Finish
	![Alt](/5_9.png "")
12. Start the VM by right clicking on the VM and clicking **Start**
	![Alt](/5_10.png "")
12. If the console is not working, use Firefox (At the time of this guide update, Chrome does not support Java sadly)
	* Go to **Java Control Panel** by searching Java in the start menu.
	![Alt](/5_11.png "")
	* Go to **Security** tab and click **Edit Site List...**
	![Alt](/5_12.png "")
	* Add https://hobbes.sus.mcgill.ca and https://hobbes.sus.mcgill.ca:8006 to the list and press OK
	![Alt](/5_13.png "")
	*Wait several minutes and open the console again. If console fails, open a new one. The second one should work.
	* IF above fails, use VNC Viewer
13. From console, **Install** -> **English** -> **Canada** -> **American English** (Keyboard)
	![Alt](/6.png "")
	![Alt](/7.png "")
	![Alt](/8.png "")
	![Alt](/9.png "")
14. Your hostname should be in the format **your-new-vm.science.mcgill.ca**.
15. Ask server admin for the root password to be used.
16. Create your personal non-admin account.
17. **Eastern** (Time Zone)
	![Alt](/10.png "")
18. Partition disk to use **entire disk**
	* Choose different partitioning schemes depending on your preference
	![Alt](/11.png "")
	![Alt](/12.png "")
	![Alt](/13.png "")
	![Alt](/14.png "")
	![Alt](/15.png "")
19. To configure the Package Manager, choose **Canada** -> **ftp.ca.debian.org**
	![Alt](/16.png "")
	![Alt](/17.png "")
20. You shouldn't need to use a HTTP proxy so leave the field blank
	![Alt](/18.png "")
21. Choose the right software based on your needs.
	* **Recommendation**: Uncheck Debian desktop environment and check SSH server
22. This should be the only OS on the VM so install the GRUB boot loader to the master boot record at **/dev/sda**.
	![Alt](/21.png "")
	![Alt](/22.png "")
23. Yay! You're done the OS installation. Restart the VM and terminal should appear for you to login using your personal account.
	![Alt](/23.png "")
24. Now we need to configure network. Get the MAC address under Hardware tab -> double click Network Device -> copy MAC Address.
	![Alt](/24.png "")
	![Alt](/25.png "")
25. SSH to dhcp.sus.mcgill.ca. If you do not have access or sudo access, ask your server admin.
26. Run `sudo nano /etc/hosts.mcconnell`.
27. 


•	configure network : DHCP and DNS
o	get MAC address under Hardware Network Device , copy it 
o	then ssh dhcp.sus.mcgill.ca
o	sudo nano /etc/hosts.mcconnell
o	assign unique IP address, ping to make sure, ping 132.216.25.86
o	add host staging {
o	hardware ethernet mac address
o	fix-address: 132.216.25.86
o	}
o	Now set up dns 
o	cd /var/lib/bind
o	vi sus.mcgill.ca.hosts
o	create a new A record: staging.sus.mcgill.ca. (must add the last period )
o	update series number
o	then sudo service bind 9 restart
o	then restart server on dhcp: sudo serive isc-dhcp-server restart
o	now open the console, then install, then change location and language,  and it should be the new VM
o	domain name : sus.mcgill.ca
o	root password: blank 
o	add first user
o	 then Guide-entire dish… enter… enter …finish! Then pick Yes
o	then pick mirror country Canada , then choose …rafal.ca
		(No need for proxy )
o	 then configuring apt, let it install , if thing pop , choose yes
o	software election, use space bar to select and de-select, uncheck Debian, check web , ssh , and standard system -> continue
o	give them all access 
o	yes to Grub , pick /dev/…
o	before,go to Hardware, click cd/dvd  choose do not…
o	Back to console, then continue! yes to really want to shut down
o	Reboot will start
➢	after, u can ssh to zili@staging.sus.mcgill.ca
if it is not staging.sus.mcgill.ca
maybe sth wrong with DNS conf
go back to dns.sus.mcgill.ca
