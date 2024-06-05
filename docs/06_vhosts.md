# vHosts

## Before we start

### What are we going to do exactly?

We will configure our Apache2 config file to enable and use virtual hosts. After that, we will be able to assign different URLs like `foo.lan`, `bar.lan`, etc. to different project directories inside the `Sites` directory.

Next, we will split up the configs for virtual hosts from one file into multiple files which can be placed inside a project directory. With this technique you’ll in the future have a better overview when you have lots of web projects inside the `Sites` directory.

Enough chit-chat let’s start hacking 👨‍💻

### Preparation

Open those files in your preferred editor:

```
/usr/local/etc/httpd/httpd.conf
/usr/local/etc/httpd/extra/httpd-vhosts.conf
/etc/hosts
```

Create new project directories inside your Sites:

```
Sites/
|  _conf/
|  _logs/
|  foo/
|  bar/
```

## Creating first Virtual Hosts

We’re going to create our first two virtual hosts. Both will have a custom URL and document root. The URLs will be `foo.lan` and `bar.lan`. The document roots will be `~/Sites/foo/public_html` and `~/Sites/foo/public_html`.

### httpd.conf

Uncomment/remove the # before the vhost_alias module:

```
#LoadModule vhost_alias_module lib/httpd/modules/mod_vhost_alias.so
```

Uncomment/remove the # before the virtual hosts inclusion:

```
#Include /usr/local/etc/httpd/extra/httpd-vhosts.conf
```

### httpd-vhosts.conf

Replace all current configs with this one here:

```
<VirtualHost *:80>
	DocumentRoot "/Users/YOUR_USERNAME/Sites/foo"
	ServerName foo.lan
	ErrorLog "/Users/YOUR_USERNAME/Sites/_logs/foo_error_log"
	CustomLog "/Users/YOUR_USERNAME/Sites/_logs/foo_access_log" common
</VirtualHost>
<VirtualHost *:80>
	DocumentRoot "/Users/YOUR_USERNAME/Sites/bar"
	ServerName bar.lan
	ErrorLog "/Users/YOUR_USERNAME/Sites/_logs/bar_error_log"
	CustomLog "/Users/YOUR_USERNAME/Sites/_logs/bar_access_log" common
</VirtualHost>
```

### hosts

Go and add at the end of your host file your new URL and let it show to your localhost:

```
127.0.0.1 foo.lan
127.0.0.1 bar.lan
```

After those edits, restart your apache with...
```
sudo apachectl restart
```

... now open [http://foo.lan/](http://foo.lan/) & [http://bar.lan/](http://bar.lan/) in your browser and you should see “**Index of** /”. Yay 🎉, we have just created our first virtual hosts! You can now place custom HTML and PHP files inside each project.

## Splitting up vHosts configs

We now will move our virtual hosts configs from the httpd-vhosts.conf file into multiple files under our projects. Why are we doing this again? Because when you’re a busy web developer you will have lots of projects and get sooner or later a little mess inside that config file. That's why I recommend to split it up.

### Custom vHosts files

Create under `Sites` inside your project directories `foo` & `bar` new config files with the name `httpd-vhost.conf` and with the related vHosts config which we have already created above.

#### First file: ~/Sites/foo/httpd-vhost.conf

```
<VirtualHost *:80>
    DocumentRoot "/Users/YOUR_USERNAME/Sites/foo/public_html"
    ServerName foo.lan
    ErrorLog "/Users/YOUR_USERNAME/Sites/foo/logs/error_log"
    CustomLog "/Users/YOUR_USERNAME/Sites/foo/logs/access_log" common
</VirtualHost>
```

#### Second file: ~/Sites/bar/httpd-vhost.conf

```
<VirtualHost *:80>
    DocumentRoot "/Users/YOUR_USERNAME/Sites/bar/public_html"
    ServerName bar.lan
    ErrorLog "/Users/YOUR_USERNAME/Sites/bar/logs/error_log"
    CustomLog "/Users/YOUR_USERNAME/Sites/bar/logs/access_log" common
</VirtualHost>
```

### Update main httpd-vhosts.conf

Remove the configs we now have moved to their own httpd-vhost.conf files. After that, we include those files but instead of including every file with their full path we’re making an optional include where we’re looking in every directory under Sites for an httpd-vhost.conf file.
Do it like that:

IncludeOptional /Users/YOUR_USERNAME/Sites/*/httpd-vhost.conf
Restart your apache with sudo apachectl restart and everything should work just fine like before.

Last notes
Creating new Virtual Host
Just add a new project folder inside the Sites directory. The folder should contain a public_html & logs directory and an httpd-vhost.conf file. You can take foo and bar as an example.

After adding your new project folder with all required subdirectories and files, don’t forget to update your hosts file. You always need to add an entry which points to your localhost (127.0.0.1).





