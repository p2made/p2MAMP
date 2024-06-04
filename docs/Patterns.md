# Patterns

* Domain names will be reversed, undersocre separated, & without the TLD, for folder names, & files name parts in `conf` & `log` files.
* The TLD will be replaced by a local development TLD (currently 'lan').
* For Yii2 sites the folder will be the domain name base, with sub-folders for ends.
* For other sites the folder will be pattern above.
* Conf files are at `/usr/local/etc/httpd/sites`, with an alias at `p2MAMP/_conf`.
* Log files are at `/usr/local/var/log/httpd`, with an alias at `p2MAMP/_logs`.



