# Setup Local (L)AMP Stack on Mac with Homebrew

I know, there are plenty of tutorials around on how to set up a LAMP stack (Linux, Apache, MySQL, PHP) on Mac with Homebrew but in this one, I’ll try to keep it short and sweet.

## Before we start

### Requirements

- 💻 macOS
- 🍺 [Homebrew](https://brew.sh/)

### Preparation

Create a Sites folder in your home directory. This folder will later be our document root, where we place our HTML and PHP files.

```
mkdir ~/Sites
```

Also, create a simple index.html file so we can later test if our apache configurations are working.

```
echo "Hello from Sites" >~/Sites/index.html
```

Type...

```
whoami
```

... into your terminal and it will return your username, we need to now that and will use it later.

## A for Apache 🦅

### Installation

Stop and unload the mac system build in apache

```
sudo apachectl stop
sudo launchctl unload -w /Systems/Library/LaunchDaemons/org.apache.httpd.plist

```

Install the [httpd formula](https://formulae.brew.sh/formula/httpd) with Homebrew (V2.4.37) (*httpd* is the same as *Apache2*)

```
brew install httpd
```

Start background service and start Apache

```
brew services start httpd
sudo apachectl start
```

When opening now [http://localhost:8080/](http://localhost:8080/) you should see “**It works!**”

### Configuration

Open in your favourite editor the file `/usr/local/etc/httpd/httpd.conf`
Find and replace configs:

`from` | `to`
------ | ----
`Listen 8080` | `Listen 80`
`DocumentRoot "/usr/local/var/www"` | `DocumentRoot "/Users/YOUR_USERNAME/Sites"`
`<Directory "/usr/local/var/www">` | `<Directory "/Users/YOUR_USERNAME/Sites">`
`AllowOverride None` * | `AllowOverride All `
`#LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so` | `LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so `
`User _www ` | `User YOUR_USERNAME `
`Group _www ` | `Group staff `
`#ServerName www.example.com:8080` | `ServerName localhost:80 `

\* Inside the previously edited `“<Directory “/Users/YOUR_USERNAME/Sites”>”`

Yeehaw! Apache is now installed and configured. After saving your changes in the httpd.conf file you need to restart your apache server with `sudo apachectl restart`. After that you can open [http://localhost](http://localhost) and should see the documents from your Sites directory.

What’s next? In my next tutorial “[Create vHosts for multiple local URLs with Homebrew Apache2/httpd](https://medium.com/@JanFaessler/create-vhosts-for-multiple-local-urls-with-homebrew-apache2-httpd-97d4ec59e125)” you can learn more about how to create multiple local URLs with different document roots.

## P for PHP 🐘

### Installation

Install the php formula with Homebrew. (V7.2.12)

```
brew install php
```

Start PHP background service

```
brew services start php
```

Configuration
Open in your favourite editor the file /usr/local/etc/httpd/httpd.conf
Find and replace configs:

Insert after the last LoadModule (probably mod_rewrite.so):

```
LoadModule php_module /usr/local/opt/php/lib/httpd/modules/libphp.so
```

Change `DirectoryIndex index.html` to `DirectoryIndex index.php index.html`

Insert after the previously edited `DirectoryIndex` area and before the htaccess/htpasswd `IfModule` area:

```
<FilesMatch \.php$>
SetHandler application/x-httpd-php
</FilesMatch>
```

Restart your Apache server...

```
sudo apachectl restart
```

Cowabunga! The latest PHP version is now installed and ready to use. Go to your Sites directory and add a info.php file with `<?php phpinfo();` in it and open it in your browser via [http://localhost/phpinfo.php](http://localhost/phpinfo.php).

What’s next? In my next tutorial “Install and run multiple PHP versions with Homebrew” you can learn how to install multiple PHP versions and run different versions on different vHosts at the same time.

## M for MySQL 🐬

### Installation

Install the mysql formula with Homebrew. (V8.0.12)


```
brew install mysql
```

Start the Homebrew MySQL daemon

```
brew services start mysql

```

### Configuration

Run...

```
mysql_secure_installation
```

... to start configuring MySQL

Now go through the procedure and set the configs as required.

I suggest this config:
```

“Would you like to setup VALIDATE PASSWORD component?”: n
“password for root”: 1234
“Remove anonymous users?”: y
“Disallow root login remotely?”: y
“Remove test database and access to it?”: y
“Reload privilege tables now?”: y
```

Hurray! MySQL is now installed and configured. You can start, stop and restart it with the Homebrew services command like this: $ brew services start|stop|restart mysql. If you’d like to test to connect PHP with your MySQL you can do it like described on this [W3Schools Site](https://www.w3schools.com/php/php_mysql_connect.asp).

What’s next? In my next tutorial “Connect MySQL to phpMyAdmin or Sequel Pro” you can learn more about how to set up and connect phpMyAdmin or Sequel Pro to your MySQL.

## What’s next?

### Links to configs

At this point, I recommend creating short links inside your Sites directory to the different config files and folders of your (L)AMP stack.

Create new etc folder inside your Sites directory

```
mkdir /Users/YOUR_USERNAME/Sites/etc

```

Apache

```
ln -s /usr/local/etc/httpd /Users/YOUR_USERNAME/Sites/etc/httpd
```

MySQL

```
ln -s /usr/local/etc/my.cnf /Users/YOUR_USERNAME/Sites/etc/my.cnf
```

PHP
```

ln -s /usr/local/etc/php/7.2 /Users/YOUR_USERNAME/Sites/etc/php72
```

### There is more

Like mentioned after each chapter I’ve made tutorials on how to improve the current state, it will be most comfortable going through them in this order:

* [Create vHosts for multiple local URLs with Homebrew Apache2/httpd](./vHosts.md)
* Coming soon: Install and run multiple PHP versions with Homebrew
* Coming soon: Connect MySQL to phpMyAdmin or Sequel Pro

--

