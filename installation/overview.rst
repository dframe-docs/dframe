.. title:: Installation - How to install dframe

.. meta::
    :description: Installation - How to install dframe - dframeframework.com
    :keywords: dframe, instalation, composer, github, download, chmod, dframeframework
    
.. meta:: property
    :og:locale: en_EN


Setup Project
===========


Requirements
----------

Requirements

 - PHP =< 7.0
 - Rewrite Module (nginix/apache2)
 - Composer
 
Instalation
----------

From console level, launch the command composer* 

.. code-block:: bash

 $ composer create-project dframe/dframe-demo appName

Or 

.. code-block:: bash

 php composer.phar create-project dframe/dframe-demo appName

**Composer** is a tool for dependency management in PHP `(You can install from here) <https://getcomposer.org/download/>`_. It allows you to declare the libraries your project depends on and it will manage (install/update) them for you.

It will initialize the process of downloading the latest version of code available on github.com

You can also enter the repository and download the code in `Dframe-demo.zip <https://github.com/dframe/dframe-demo/releases>`_. and run:

.. code-block:: bash

 $ composer install

After that you should have new project app skeleton.

Directory Permissions
----------

Not all folders exist at the start, but there are some. All files and folders, except for the ones listed below, should have |chmod755| for folders and |chmod664| for files. Does not apply to the ones listed below, which should have group permissions of |www-data|

.. code-block:: bash

 chmod 777 -R app/View/cache
 chmod 777 -R app/View/templates_c
 chmod 777 -R app/View/uploads
 
 
.. |chmod755| cCode:: chmod 755
.. |chmod664| cCode:: chmod 664
.. |www-data| cCode:: www-data

 
HTTP Server
----------

After installing, you should configure web server's document /web. Make sure you have loaded mod_rewrite



.. customLi:: myTab
 :apache2: Apache (.htaccess)
 :nginx: active/Nginx (.conf)
 
  .. code-block:: apache
  
   RewriteEngine On
   
   #Deny access for hidden folders and files
   RewriteRule (^|/)\.([^/]+)(/|$) - [L,F]
   RewriteRule (^|/)([^/]+)~(/|$) - [L,F]
   
   #Set root folder to web directory
   RewriteCond %{REQUEST_FILENAME} !-d
   RewriteCond %{REQUEST_FILENAME} !-f
   RewriteRule ^(.*)$ web/$1
   
   #Redirect all queries to index file
   RewriteCond %{REQUEST_FILENAME} !-f
   RewriteRule ^(.*)$ web/index.php [QSA,L]
  next
  
  .. code-block:: nginx
  
   #Set root folder to web directory
   location / {
       root   /home/[project_path]/htdocs/web;
       index  index.html index.php index.htm;
       if (!-e $request_filename) {
           rewrite ^/(.*)$ /index.php?q=$1 last;
       }
   }
   
   #Redirect all queries to index file
   location ~ .php$ {
       try_files $uri = 404;
       fastcgi_pass 127.0.0.1:9000;
       #fastcgi_pass unix:/run/php/php7.1-fpm.sock;
       fastcgi_index web/index.php;
       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       include fastcgi_params;
   }

