# Setting Up LAMP Stack in macOS (The Hard Way)

5. Configure the httpd-userdir.conf File
6. Enable PHP 8.0
7. Is PHP working properly now?
8. Install MySQL
9. Install phpMyAdmin

--

## Step 7: Install MySQL

Visit the [MySQL downloads](https://dev.mysql.com/downloads/mysql/) page & download the DMG Archive installer. On the page that clicking for the download takes you to, click **No thanks, just start my download.**

Proceed with the installation up until you reach **Configuration** stage (this should be the second last part of the installation process).

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

