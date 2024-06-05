# PHP 🐘

## Installation

Install the php formula with Homebrew.

```
brew install php
```

Start PHP background service

```
brew services start php
```

## Configuration

Open `/usr/local/etc/httpd/httpd.conf` in your favourite editor.

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

--
<!-- 03 PHP -->

[Apache](02_Apache.md) |
[README](../README.md) |
[MySQL](04_MySQL.md)

--

