change hostname to ael-hadj42
this is a test
# What is virtual box?
It is a software that enables us to run other operating systems on the current machine and os we are running without interfering with it
so what does it contain :
- isolation, virtualization, cross, control panel 
I will be using debian since it is far to easy to install rather than rocky which requires a bit more experience
## requirements
- App armor is a must at startup
- at least 2 encrypted partitions using LVM. **![[Pasted image 20241122051322.png]]
- Learn about:
	- apt vs aptitude
	- SELinux
	- Apparmor
	- understand anything you use
	- ssh
	- hostname : ael-hadj42, must be able to modify it during eval
	- implement strong password policy *last thing to see i guess*
	- install and configure sudo with strict rules
	- users : root + ael-hadj42
	- This user has to belong to the user42 and sudo groups.
	- During the defense, you will have to create a new user and assign it to a group
	- Password:
		- password must expire every 30 days
		- min day allowed before modification of password is 2
		- warning message before password expires is 7 days
		- password is 10 characters, must contain upper/lower case number no more than 3 consecutive indetical characters
		- must not include name of the user
		- The following rule does not apply to the root password: The password must have at least 7 characters that are not part of the former pasword.
		- 