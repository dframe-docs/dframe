.. title:: Installation - How to install dframe

.. meta::
    :description: Installation - How to install dframe - dframeframework.com
    :keywords: dframe, instalation, composer, github, download, chmod, dframeframework   

Download
----------

From console level, launch the command

.. code-block:: bash

 $ composer create-project dframe/dframe-demo appName

alternatively

.. code-block:: bash

 php composer.phar create-project dframe/dframe-demo appName

It will initialize the process of downloading the latest version of code available on github.com

Or you can enter the repository and download the code in Dframe-demo.zip.

Permissions
----------

Not all folders exist at the start, but there are some. All files and folders, except for the ones listed below, should have |chmod755| for folders and |chmod664| for files. Does not apply to the ones listed below, which should have group permissions of |www-data|


.. code-block:: bash

 chmod 777 -R app/View/cache
 chmod 777 -R app/View/templates_c
 chmod 777 -R app/View/uploads

.. |chmod755| cCode:: chmod 755
.. |chmod664| cCode:: chmod 664
.. |www-data| cCode:: www-data

