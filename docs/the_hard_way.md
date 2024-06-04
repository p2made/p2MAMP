# Setting Up LAMP Stack in macOS (The Hard Way)

3. Create username.conf File
4. Configure the httpd.conf File
5. Configure the httpd-userdir.conf File
6. Enable PHP 8.0
7. Is PHP working properly now?
8. Install MySQL
9. Install phpMyAdmin

--

## Setup `username.conf`

To be able to recognize the files stashed inside the `Sites` directory, you will require setting up a configuration file called `<username>.conf` where `<username>` is the result of the `whoami` lookup performed in step 1.

Run...

```
cd /usr/local/etc/httpd
```

We'll create a `users` directory, & in preparation for steps ahead, a `sites` directory.

```
mkdir ./users
mkdir ./sites
```


Check to see if there is an existing <username>.conf file (it will appear in the Terminal window if it exists). If so, make a backup copy by entering the following command:


sudo cp <username>.conf <username>.conf.bak
Remember to replace all instances of <username> with your own account username!

Extra Note: sudo is used to request administrator access in a UNIX system. Often if you use sudo, you'll be prompted to enter your password. Don't worry, this is normal.

We are now ready to create/edit our configuration file. Type sudo nano <username>.conf in the Terminal window and press Enter. If the file does not exist, you should see a blank window space.

Extra Note: nano is a text editor used in UNIX command line interface (CLI) systems that functions like any regular text editor you've used before, just without mousr support and some nice-to-have features. Frequent UNIX users will find themselves using it often. Alternatives are vi and vim, and I think there's a debate among some nerds about which is best. Unless this piques your interest, you need not know the details behind it.

Copy and paste the following configuration.


<Directory "Users/<username>/Sites/">
AllowOverride All
Options Indexes MultiViews FollowSymLinks
Require all granted
</Directory>
A gentle reminder: Remember to replace all instances of <username> with your account username!

Press Ctrl+O and press Enter to save the file.

Press Ctrl+X and press Enter to exit the nano editor.

Step 4: Configure the httpd.conf File
Now there's also another configuration file we'll need to modify, namely the httpd.conf file.

In your Terminal window, enter in


cd /etc/apache2
Type ls and press Enter again. You should see httpd.conf being listed as one of the files inside the current directory.

Type sudo cp httpd.conf httpd.conf.bak and press Enter. This will create a backup of this configuration file in case something goes awry.

Type sudo nano httpd.conf and press Enter. You should see something like this appear in the nano text editor in your Terminal window.

httpd.conf File

Press Ctrl+W, type in LoadModule authz_core_module and press Enter. We are using a search feature in the nano text editor to look for specific lines inside this file.

httpd.conf File Search

Uncomment the following modules. The # you see in front of each line means that line is commented out (i.e., that specific line or module will be ignored). What we want to do is to enable these modules in our Apache server.


LoadModule authn_core_module libexec/apache2/mod_authn_core.so

LoadModule authz_host_module libexec/apache2/mod_authz_host.so

LoadModule userdir_module libexec/apache2/mod_userdir.so

LoadModule include_module libexec/apache2/mod_include.so

LoadModule rewrite_module libexec/apache2/mod_rewrite.so
Uncomment the following line for the User home directories.


Include /private/etc/apache2/extra/httpd-userdir.conf
Press Ctrl+W, search up DocumentRoot and press Enter. We want to edit the following two lines – this shows DocumentRoot configuration in httpd.conf which dictates the folder where your machine will source its web files to be used inside localhost.


...
DocumentRoot "/Library/WebServer/Documents"
<Directory "/Library/WebServer/Documents">
...
You may choose to replace them completely with these two lines or add them after commenting the latter.


...
DocumentRoot "/Users/<username>/Sites/"
<Directory "/Users/<username>/Sites/">
...
A gentle reminder: Remember to replace all instances of <username> with your account username!

Press Ctrl+W, search up AllowOverride None and press Enter. Here, replace this line with AllowOVerride All.

Your DocumentRoot configuration in httpd.conf should look like as follows:

httpd.conf File Modified

For extra measure, we can set the Apache Server to serve an index.php file instance first before an index.html. Press Ctrl+W, search up DirectoryIndex and press Enter. You should see the following:

httpd.conf Search DirectoryIndex

From the line where it says


DirectoryIndex index.html
Add index.php so it now says


DirectoryIndex index.php index.html
Press Ctrl+O and press Enter to save the file.

Press Ctrl+X and press Enter to exit the nano editor.

Step 5: Configure the httpd-userdir.conf File
In yuor Terminal window, enter in cd /etc/apache2/extra.

Type ls and press Enter again. You should see httpd-userdir.conf being listed as one of the files inside the current directory.

Type sudo cp httpd-userdir.conf httpd-userdir.conf.bak and press Enter. This will create a backup of this configuration file in case something goes awry.

Type sudo nano httpd-userdir.conf and press Enter. You should see something like this appear in the nano text editor in your Terminal window:

httpd-userdir.conf File

Uncomment the following line.


Include /private/etc/apache2/users/*.conf
Press Ctrl+O and press Enter to save the file.

Press Ctrl+X and press Enter to exit the nano editor.

After carrying out the previous 5 steps, type in sudo apachectl restart amd press Enter. If you type in localhost or 127.0.0.1 in your web browser's address bar now, it should show a 403 Forbidden Access error akin to the following:

403 Forbidden Access

Not to worry! It means everything's been set up properly so far. This error is caused due to the Sites folder we created not having an index.php or index.html file to display.

Step 6: Enable PHP 8.0
macOS has PHP built in (at least in macOS Big Sur and earlier). The PHP shipped with Big Sur is PHP version 7.3.22, which Apple hints on removing in one of the upcoming future versions of macOS. Regardless, we will be using a fresh install of PHP 8.0 using an installation package manager called Homebrew.

Head over to https://brew.sh. You will be greeted with Homebrew's home page as follows:

Homebrew Home Page

The first section you should see is "Install Homebrew", where there's a Terminal command line following the title. Copy that command line and run it in a Terminal window. This will install Homebrew inside your Mac. Give it time, it will be ready for use shortly.

After installing Homebrew, type in brew install php. This will install PHP as well as any other software dependencies required into your Mac (think of software dependencies as pre-requisites).

Open a new tab in the Terminal (or reboot the Terminal application). If you enter in php --version, you should view the PHP version being used like as follows:

PHP Info in Terminal

NOTE ABOUT THIS TERMINAL WINDOW INTERFACE
I'm currently using Oh My Zsh in my terminal, this is not required for you to follow this whole guide. If you would like to try out my not-so-custom Oh My Zsh Terminal theme, look up how to install Oh My Zsh first, then head over to the following link to download my theme. Have fun either way!

My Custom Theme Here

Now that PHP's been installed in your Mac, we will need Apache to use this PHP by default. To do this, we will have to return to our httpd.conf file once more to make some further changes.

In your Terminal window, enter in sudo nano /etc/apache2/httpd.conf.

You may have noticed that after successfully installing PHP with Homebrew. If you've missed it, you can type in brew info php and the instructions will reappear again.

brew info php

Refer to the part under Caveats. This part details what is required to add into your httpd.conf file.

Depending on your Mac (from what I've known from M1 Mac users, the file locations may vary), you may be asked to insert the following or a variant of it:


 LoadModule php_module /usr/local/opt/php/lib/httpd/modules/libphp.so

 <FilesMatch \.php$>
     SetHandler application/x-httpd-php
 </FilesMatch>
That is all which will be needed

Press Ctrl+W, search up LoadModule php and press Enter. After the last LoadModule line, paste the required lines as mentioned in Line 7.

Danger

Do not uncomment the following line!


#LoadModule php7_module libexec/apache2/libphp7.so
Uncommenting this will get Apache to run the outdated PHP that came with your Mac, we don't want this.

Press Ctrl+O and press Enter to save the file.

Press Ctrl+X and press Enter to exit the nano editor.

Type in sudo apachectl restart and press Enter. This should enable the changes we made to have Apache run PHP 8.0 in your Mac.

Is PHP working properly now?
To check if PHP is working on your local Mac's web server, we will be creating a phpinfo() file to load into your browser.

Go to your Sites folder and create a file called phpinfo.php.

In phpinfo.php, put in the following code and save the file:

phpinfo.php

<?php phpinfo(); ?>
If you type in localhost/phpinfo.php or 127.0.0.1/phpinfo.php in your web browser's address bar, you should see something like this:

phpinfo.php

Congratulations, you're now running a current version of PHP on your local web server!

Step 7: Install MySQL
You will also require a MySQL or mariaDB database server. This step will involve installing MySQL's Community Server inside your Mac to use – if you plan on learning more on MySQL databases, consider this a way to kill two birds with one stone! 😁

Like with PHP, you can install MySQL or mariaDB with Homebrew. However, the following instructions will focus more towards not interacting with the Terminal as much as possible.

Head over to the following link and download the DMG Archive installer.

https://dev.mysql.com/downloads/mysql/

Proceed with the installation up until you reach Configuration stage (this should be the second last part of the installation process).

At the Configuration stage, you will be required to select a Password Encryption type. We will be installing one more piece of software to access our MySQL database later, but it will not work well if we choose the Strong Password Encryption option. In this case, select Use Legacy Password Encryption.

MySQL Password Encryption Settings

Enter in your MySQL database password.

Enter MySQL Password

OOPS!

If you proceeded with the Strong Password Encryption by mistake during installation, don't worry. Head over to System Preferences, and you should see MySQL as one of the last options in the window as follows:

System Preferences

Select the MySQL option and you will be greeted with a dashboard like as follows:

MySQL Dashboard

Select Initialize Database and you'll be greeted with this screen.

MySQL Initialize Database

Select Use Legacy Password Encryption and enter your password again. Crisis averted!! 😌

Open up a NEW Terminal window and type in ls -a. Look and see if either a file called .zshrc or .bashrc exists inside your account folder. In this example, my machine has .zshrc.

Search for .zshrc or .bashrc

If you have .zshrc like in my case (this should be true for all those running macOS Cataline or later), type the following command:


echo "export PATH=${PATH}:/usr/local/mysql/bin/" >> ~/.zshrc
If you're using .bashrc instead, type the following command:


echo "export PATH=${PATH}:/usr/local/mysql/bin/" >> ~/.bashrc
Open a new Terminal window and enter in mysql --version. It should print out the version of MySQL installed in your machine.

MySQL Version Info

Type in mysql -u root -p. You should be prompted to enter the password you've set for MySQL. Once you've entered the correct password, you should see a CLI nterface to enter MySQL commands. We don't need to do anything here for now, so just type in exit and press Enter to exit this interface.

MySQL CLI

If you like, you can also proceed to install MySQL Workbench. It is an IDE that is primarily used to create and test MySQL scripts. However, we will be using something else instead for this module.. and that is phpMyAdmin.

Step 8: Install phpMyAdmin
Alright, the last thing you'll need to install and/or configure.. I promise! Also like PHP, you can install phpMyAdmin using Homebrew, but we will again avoid using the Terminal like for MySQL in the following instructions.

Head over to the following link to download phpMyAdmin (which is in a zip file): https://phpmyadmin.net

Unzip the zip file. Rename the folder to just "phpmyadmin" and move it to the Sites folder.

If you type in localhost/phpinfo.php or 127.0.0.1/phpinfo.php in your browser's address bar, you should be greeted with a login page like this: phpMyAdmin Login

Type in root as your username and enter the password you set when installing MySQL earlier. You should now be greeted with a control panel like this: phpMyAdmin Dashboard

And... you're done! That is all you need to install and configure to prepare a LAMP stack from scratch. I hope you've gained a sense of accomplishment from going through these steps, it's a fruitful one!

If you have any more questions, you know where to reach me. 😉

References
[Tech CookBook] Setting Up Your Local Web Server on macOS Big Sur 11.0.1 (2020) | MAMP | macOS, Apache, MySQL, PHP. Link: https://tech-cookbook.com/2020/11/14/setting-up-your-local-web-server-on-macos-big-sur-11-0-1-2020-mamp-macos-apache-mysql-php/

[YouTube] How To Set Up phpMyAdmin With MySQL 8.0+ on MacOS. Link: https://youtu.be/SVNbRXUEDUg

PreviousXAMPP Installation
NextHTML Deprecated Tags & Attributes
Copyright © 2021 - 2022 Henry Heng
Made with Material for MkDocs

