# 2 Apache 🦅

## Installation

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

Open your web browser and type [http://localhost:8080](http://localhost:8080) or [http://127.0.0.1:8080](http://127.0.0.1:8080) in the address bar. If the Apache Server has successfully started, you should see this appear in your web browser:

![It works!](img/lamp_001.png)

## Create Sites Directory

The page you're looking at now is a result of a default webpage file that is in the following location:

```
/usr/local/var/www
```

We will create a Sites directory under your home folder and use that folder to contain all our PHP web files. At the end of this guide, typing in [http://localhost](http://localhost) or [http://127.0.0.1](http://127.0.0.1) in your web browser's address bar will source the web files from this folder for view.

Head over to your account folder. Create a folder named Sites. Upon creating the folder, the folder icon should now have a compass image appear on top of it like as shown below:

![Sites folder](img/lamp_002.png)

## Setup `username.conf`

To be able to recognize the files stashed inside the `Sites` directory, you will require setting up a configuration file called `<username>.conf` where `<username>` (& elsewhere in this guide) is the result of the `whoami` lookup performed in step 1.

Run...

```
cd /usr/local/etc/httpd
```

(Until told otherwise, all Terminal commands run from here.)

We'll create a `users` directory, & in preparation for steps ahead, a `vhosts` directory.

```
mkdir ./users
mkdir ./vhosts
```

In your favourite editor create this `<username>.conf` with the following contents...

```
<Directory "Users/<username>/Sites/">
	AllowOverride All
	Options Indexes MultiViews FollowSymLinks
	Require all granted
</Directory>
```

To create `<username>.conf` as an empty file...

```
touch ./users/<username>.conf
```

## Configure `httpd.conf`

Now there's also another configuration file we'll need to modify, namely the `httpd.conf` file. First back it up...

```
cp ./httpd.conf ./httpd.conf.bak
```

Open in your favourite editor the file `/usr/local/etc/httpd/httpd.conf`

Find and replace configs:

Dir | Change
------ | ----
**From** | `Listen 8080`
**To**   | `Listen 80`
**From** | `User _www`
**To**   | `User <username>`
**From** | `Group _www`
**To**   | `Group staff`
**From** | `#ServerName www.example.com:8080`
**To**   | `ServerName localhost:80`
**From** | `DirectoryIndex index.html`
**To**   | `DirectoryIndex index.php index.html`

Uncomment the following lines...

```
LoadModule userdir_module lib/httpd/modules/mod_userdir.so
LoadModule include_module lib/httpd/modules/mod_include.so
LoadModule rewrite_module lib/httpd/modules/mod_rewrite.so
```


Uncomment the following line for the User home directories.

```
Include /private/etc/apache2/extra/httpd-userdir.conf
```

Replace these 2 lines...


```
DocumentRoot "/usr/local/var/www"
<Directory "/usr/local/var/www">
```

With...

```
DocumentRoot "/Users/<username>/Sites"
<Directory "/Users/<username>/Sites">
```

Inside that directory block, change...

Dir | Change
------ | ----
**From** | `AllowOverride None`
**To**   | `AllowOverride All`

## Configure `httpd-userdir.conf`

Backup the `httpd-userdir.conf` file...

```
cp ./extra/httpd-userdir.conf ./extra/httpd-userdir.conf.bak
```

Open `/usr/local/etc/httpd/extra/httpd-userdir.conf` in your favourite editor.

Dir | Change
------ | ----
**From** | `UserDir public_html`
**To**   | `UserDir Sites`
**From** | `<Directory "/home/*/public_html">`
**To**   | `<Directory "/Users/*/Sites">`

Immediately above the directory line, add...

```
Include /private/etc/apache2/users/*.conf
```

--
<!-- 02 Apache -->

[Preparation](01_Preparation.md) |
[README](../README.md) |
[PHP](03_PHP.md)

--

