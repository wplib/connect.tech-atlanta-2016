#Easy Local Development

**_For WordPress (and PHP)_**

_Presented by Mike Schinkel to Connect.Tech Atlanta October 2016_


#Instructions for Windows users using Powershell

##1. Install VirtualBox
	https://www.virtualbox.org/wiki/Downloads
	
##2. Install Vagrant
    https://www.vagrantup.com/downloads.html
    
##3. Install Vagrant Plugins
- Vagrant Hosts Updater:

        vagrant plugin install vagrant-hostsupdater

- Vagrant Triggers:

	    vagrant plugin install vagrant-triggers

##4. Pick your _"Sites"_ Directory

	Set-Location -Path c:$env:userprofile
	
	New-Item -ItemType Directory -Path Sites -Force 

##5. Get WPLib Box
### _Download_ if you DO NOT have Git installed:

	Set-Location -Path c:$env:userprofile\Sites
		
	wget https://github.com/wplib/wplib-box/archive/ master.zip \
		-OutFile wplib.box.zip 
	
	Expand-Archive -LiteralPath wplib.box.zip \
		-DestinationPath wplib.box
	
	Move-Item wplib.box\wplib-box-master\* wplib.box
	
	Remove-Item wplib.box\wplib-box-master


### _Clone_ if you have Git installed:

	Set-Location -Path c:$env:userprofile\Sites
	
    git clone https://github.com/wplib/wplib-box.git \ 
    	wplib.box
    
    Set-Location -Path wplib.box

##6. Now: Vagrant Up!
Now just run: 

	vagrant up

This should only take 4-5 minutes, mostly to download the actual box image file itself.

##7. Open wplib.box
Next open http://wplib.box in your browser.

##8. Login to the Admin Console

Login to the `/wp-admin/` and look around

- Username: `admin`
- Password: `admin`

##9. Change the Domain Name

1. Change the directory name from `wplib.box` to `example.box`:

		Set-Location -Path c:$env:userprofile\Sites
		Rename-Item wplib.box example.box

2. Open `Vagrantfile` from `../Sites/example.box/` in your editor.

3. Find these lines:

    	config.vm.hostname = "wplib.box"
	    config.hostsupdater.aliases = [
    	    "adminer.wplib.box",
        	"mailhog.wplib.box"
	    ] 

4. Replace `wplib.box` with `example.box`.
5. Return back to Command line in `Sites/example.box/` directory
6. Run:

   		vagrant reload
   		
7. Open http://example.box in your browser.


##10. Exploring the Database

1. Open http://adminer.example.box in your browser
2. Enter credentials:

- Username: `wordpress`
- Password: `wordpress`
- Database: `wordpress`

3. Look around!

##11. Debugging with PhpStorm

See [Debugging with PhpStorm and XDEBUG](debugging-with-phpstorm-xdebug.md).

##12. Fixing IP Conflicts

- Open the file named `IP` in `Sites/example.box/`.
- Change it if you need to

##13. Importing Your Own Project

1. Edit `Vagrantfile` to use your own domain name i.e. `mysite.dev` instead of `example.box`
1. Export your own MySQL database to `Sites/example.box/sql/mysite.sql`
2. Delete the contents of the `www` directory
3. Copy your code into the `www` directory
4. Update `DB_USER`, `DB_PASSWORD` and `DB_NAME` in `wp-config.php` to be:
	- Username: `wordpress`
	- Password: `wordpress`
	- Database: `wordpress`
5. Run `vagrant ssh`
6. Run `box import-db mysite.sql`	
7. Type `exit`
8. Open `example.dev` in your browser






