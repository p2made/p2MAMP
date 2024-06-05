# 4 MySQL 🐬

## Installation

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


[http://localhost/phpmyadmin/](http://localhost/phpmyadmin/)

### There is more

Like mentioned after each chapter I’ve made tutorials on how to improve the current state, it will be most comfortable going through them in this order:

* [Create vHosts for multiple local URLs with Homebrew Apache2/httpd](./vHosts.md)
* Coming soon: Install and run multiple PHP versions with Homebrew
* Coming soon: Connect MySQL to phpMyAdmin or Sequel Pro

<!-- 04 MySQL -->

[PHP](03_PHP.md) |
[README](../README.md) |
[Virtual Hosts](05_vhosts.md)

--

