# TurtlePass
Thank you for purchasing TurtlePass. TurtlePass is an easy to use team password management tool. It provides security for you and your team. You can store and share your team passwords.

## System Requirements 

TurtlePass is a web application written in PHP, so you need a default webserver stack to run it.
It should work with most webspace providers.

  - PHP 5.3+, PHP 7.0, PHP 7.1
  - MySQL 5.0+
  - Apache or Nginx web server

## Upload TurtlePass

First you need to upload all TurtlePass files to your webserver.
Most webspace providers will allow you to upload the files with a ftp service.

You should not mix the TurtlePass files with other files.
We recommend you to create a blank directory for TurtlePass.

## Setup web server

Now you need to setup your webserver.

If you are using a rent webspace or your own server with a web control panel like plesk, you need to change the domain's root directory in the control panel to the "web" directory of TurtlePass.

If you are using your own server, you need to create a vhost in your prefered web server.
We provide example vhost configuration files for apache2 and nginx which should work great with TurtlePass.

## Secure TurtlePass

To secure TurtlePass you should enable OpenSSL (https) on your webserver.

If you use a rent webspace you need follow the SSL instructions of your provider to secure the domain.

If you are using your own server with apache2 or nginx, you need to configure the TurtlePass vhost to enable ssl. You will find the necessary information in the documentation of your web server.


## Installation Instructions

TurtlePass comes with a build-in installation wizard. Follow the instructions on the screen to install TurtlePass. To open the install wizard navigate to http://your-domain.tld/install.php

At first you need to put in the login credentials for the database connection. The host and port is normally localhost and 3306. If you using a standard web hosting package, you’ll normally  find these informations in your control panel.

Second you need to provide a secret key. This key is used to encrypt your data and store secured in the database. It must be 32 characters long.

At least you need to provide your personal information. The username and password will be your login credentials for the system. Please keep this data save and secure. After fulfill this form you can click on install and the wizard will install the software. Once installed, the ``install.php`` will be automatically removed and the ``app/config/parameters.yml`` is generated. 


### Apache2 Configuration

```
<VirtualHost *:80>
    ServerName domain.tld
    ServerAlias www.domain.tld

    DocumentRoot /var/www/turtlepass/web
    <Directory /var/www/turtlepass/web>
        AllowOverride All
        Order Allow,Deny
        Allow from All
    </Directory>

    ErrorLog /var/log/apache2/turtlepass_error.log
    CustomLog /var/log/apache2/turtlepass_access.log combined
</VirtualHost>
```

### Nginx Configuration

```
server {
    server_name domain.tld www.domain.tld;
    root /var/www/turtlepass/web;

    location / {
        try_files $uri /app.php$is_args$args;
    }

    location ~ ^/(app|install)\.php(/|$) {
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.*)$;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        fastcgi_param DOCUMENT_ROOT $realpath_root;
        internal;
    }

    location ~ \.php$ {
      return 404;
    }

    error_log /var/log/nginx/turtlepass_error.log;
    access_log /var/log/nginx/turtlepass_access.log;
}
```

## Backup

To backup the system please export the entire database using an administration tool eg. phpMyAdmin. Also it is important to save the encryption key. This key is located in the app/config/parameters.yml file. You can easily copy this file. You should never store both files (database dump + parameters.yml) at the same location. With both files you can decrypt all information stored in the database.

## Bug Report

Please report all bugs to support@turtlepass.net, by adding the following information:

A clear title
Description of the bug
Reproduction steps
Environment considerations (OS, browser, hardware)
Any additional materials that make digging into the problem easier (stack traces, logs, script files, specific data sets, screen shots).


## API Reference

You find your API Documentation and RestFUL web client at http://your-domain.tld/doc. To receive an API token, please log in with your credentials. Next click in the upper right corner on your user name and choose settings. Afterwards you can click on the lefthand side on “Manage Tokens” to create a new token for the api and start integrating TurtlePass into other applications. 
