.. title:: Simple PHP Router - Dframeframework.com

.. meta::
    :description: Creating an application, it's worth taking care of their friendly links. Its has a big part in position in SEO. Link router work in a similar way as network router.
    :keywords: dframe, router, routing, urls, url, friendlyurl, htaccess, routes, dframeframework  


====
Simple PHP Router
====

Creating an application, it's worth taking care of their friendly links. Its has a big part in position in SEO. Link router work in a similar way as network router. It is responsible for calling the method from controller.

.. code-block:: php

 $this->router->addRoute([
     'page/:method' => [
         'page/[method]/', 
         'task=page&action=[method]'
     ]
 ]);
 $this->router->makeUrl('page/:action?action=index'); // Return: https://example.php/page/index
 $this->router->isActive('page/:action?action=index'); // Current Website true/false

Configuration
===========

We define the table with adresses for our application in the configuration file
 
 - |https| - true/false forcing https
 - |NAME_CONTROLLER| - Name of the default controller
 - |NAME_METHOD| - Name of the default method from the controller in NAME_CONTROLLER
 - |publicWeb| - Main folder from which files will be read (js, css)
 - |assetsPath| - Dynamic folder
 
 - |docs/docsId| - Example routing with the |docsId| variable, which contains the |docs/[docsId]/| adress definition and the |task| parameters to which it's assigned.
 - |error/404| - as above
 - |default| - default definition loading the controller/method. |params| defines the possibility of additional parameters appearing, while

.. code-block:: php

 '_params' => [
     '[name]/[value]/',
     '[name]=[value]'
 ]

defines the way the additional foo=bar parameters should be interpreted.

**Config/router.php**

.. code-block:: php

 <?php
 
 return [
     'https' => false,
     'NAME_CONTROLLER' => 'page',    // Default Controller for router
     'NAME_METHOD' => 'index',       // Default Action for router
     'publicWeb' => '',              // Path for public web (web or public_html)
 
     'assets' => [
         'minifyCssEnabled' => true,
         'minifyJsEnabled' => true,
         'assetsDir' => 'assets',
         'assetsPath' => APP_DIR.'View/',
         'cacheDir' => 'cache',
         'cachePath' => APP_DIR.'../web/',
         'cacheUrl' => HTTP_HOST.'/',
     ],
 
     'routes' => [
         'docs/:pageId' => [
             'docs/[pageId]/', 
             'task=page&action=[docsId]&type=docs'
         ],
         
         'error/:code' => [
             'error/[code]/', 
             'task=page&action=error&type=[code]',
             'code' => '([0-9]+)',
             'args' => [
                 'code' => '[code]'
             ],
         ],
         
        ':task/:action' => [
            '[task]/[action]/[params]',
            'task=[task]&action=[action]',
            'params' => '(.*)',
            '_params' => [
                '[name]/[value]/',
                '[name]=[value]'
            ]
        ],

         'default' => [
             '[task]/[action]/[params]',
             'task=[task]&action=[action]',
             'params' => '(.*)',
             '_params' => [
                 '[name]/[value]/', 
                 '[name]=[value]'
             ]
         ]
     ] 
 
 ];

Controller
-------------

 - makeUrl - is used for generating the full adress. For example |makeurl| - method used for redirections, equivalent of |header| but with a parameter being a key from the Config/router.php table. In case of using docs/:docsld it looks as the following |redirect|

**Controller/Page.php**

.. code-block:: php

 <?php
 
 namespace Controller;
 
 use Dframe\Controller;
 use Dframe\Router\Response;
 
 class PageController extends Controller
 {
 
     /**
      * @return bool
      */
     public function index()
     {
         echo $this->router->makeUrl('docs/:docsId?docsId=23');
         return;
     }
 
     /**
      * @return mixed
      */
     public function docs()
     {
 
         if (!isset($_GET['docsId'])) {
             return $this->router->redirect('error/:code?code=404');
         }
     }
 
     /**
      * @param string $status
      *
      * @return mixed
      */
     public function error($status = '404')
     {
         $routerCodes = $this->router->response();
 
         if (!array_key_exists($status, $routerCodes::$code)) {
             return $this->router->redirect('error/:code?code=500');
         }
 
         $view = $this->loadView('index');
         $smartyConfig = Config::load('view/smarty');
 
         $patchController = $smartyConfig->get('setTemplateDir', APP_DIR . 'View/templates') . '/errors/' . htmlspecialchars($status) . $smartyConfig->get('fileExtension', '.html.php');
 
         if (!file_exists($patchController)) {
             return $this->router->redirect('error/:code?code=404');
         }
 
         $view->assign('error', $routerCodes::$code[$status]);
         return Response::create($view->fetch('errors/' . htmlspecialchars($status)))->headers(['refresh' => '4;' . $this->router->makeUrl(':task/:action?task=page&action=index')]);
     }
 
 }
 
     
     
.. |router| cCode:: 
 <?php $this->router; ?>
.. |page/index| cCode:: 
 <?php $this->router->makeUrl(':task/:action?task=page&action=index'); ?>
.. |$router| cCode:: {$router}
.. |$makeurl| cCode:: {$router->makeUrl(':task/:action?task=index&action=page&page=1')}


View
-------------

assign - it's a method of the template engine that assignes value to a variable which is used in the template files.

**View/templates/index.html.php**

.. customLi:: myTabs
 :php: active/php
 :smarty: smarty

  .. code-block:: php

   <?php include "header.html.php" ?>
   Example site created using the Dframe Framework

   Routing:
   <?php $this->router->makeUrl(':task/:action?task=index&action=page'); ?> <!-- http://example.com/index/page -->
   <?php $this->router->makeUrl('error/:code?code=404'); ?> <!-- http://example.com/page/404 -->
   <?php $this->router->publicWeb('css/style.css'); ?> <!-- http://example.com/css/style.css -->

   <?php $this->domain('https://example.com')->makeUrl('error/:code?code=404'); ?> <! -- http://examplephp.com/page/404 -->

   <?php include "footer.html.php" ?>
   Using only PHP

  - |router| all already available methods used like in |page/index|

  next

  .. code-block:: php

   {include file="header.html.php"}
   Example site created using the Dframe Framework

   Routing:
   {$router->makeUrl(':task/:action?task=index&action=page')} <! -- http://example.com/index/page -->
   {$router->makeUrl('error/:code?code=404')} <!-- http://example.com/page/404 -->
   {$router->publicWeb('css/style.css')}  <!-- http://example.com/css/style.css -->

   {$router->domain('https://examplephp.com')->makeUrl('error/:code?code=404')}  <!-- http://examplephp.com/page/404 -->

   {include file="footer.html.php"}
   S.M.A.R.T.Y Engine used in the example

  - |$router| all already available methods are used like in |$makeUrl|

**View/index.php**

.. code-block:: php

 namespace View;

 use Dframe\Asset\Assetic;

 class IndexView extends \View\View
 {

     /**
      * @return bool
      */
     public function init()
     {
         $this->router->assetic = new Assetic();
         $this->assign('router', $this->router);
     }
 }

.. center::

 Dframe\Router\Response;

Extention of the basic **Dframe\Router** is **Dframe\Router\Response**, adding functionality of setting the response status (404, 500, etc.) and their headers.

.. code-block:: php

 return Response::create('Hello Word!')
     ->status(200)
     ->headers([
         'Expires' => 'Mon, 26 Jul 1997 05:00:00 GMT',
         'Cache-Control' => 'no-cache',
         'Pragma',
         'no-cache'
     ]);
     
For generating html.

Render json

.. code-block:: php

 return Response::renderJSON(['code' => 200, 'data' => []]); 

Render json with callback

.. code-block:: php

 return Response::renderJSONP(['code' => 200, 'data' => []]); 

Redirect

.. code-block:: php

 return Response::redirect(':task/:action?task=page&action=login'); 


.. |https| cCode:: https
.. |NAME_CONTROLLER| cCode:: NAME_CONTROLLER
.. |NAME_METHOD| cCode:: NAME_METHOD
.. |publicWeb| cCode:: publicWeb
.. |assetsPath| cCode:: assetsPath
.. |docs/docsId| cCode:: docs/:docsId
.. |docsId| cCode:: :docsId
.. |docs/[docsId]/| cCode:: docs/[docsId]/
.. |task| cCode:: task=page&action=docs&docsId=[docsId]
.. |error/404| cCode:: error/404
.. |default| cCode:: default
.. |params| cCode:: 'params' => '(.*)'

.. |makeUrl| cCode:: $this->router->makeUrl('docs/:docsId?docsId=23');
.. |header| cCode:: Header('Location: ""');
.. |redirect| cCode:: $this->router->redirect(':task/:action?task=index&action=page');
