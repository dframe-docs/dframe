Configuration
------------

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

 '_params' => array(
     '[name]/[value]/',
     '[name]=[value]'
 )

defines the way the additional foo=bar parameters should be interpreted.

**Config/router.php**

.. code-block:: php

 <?php
 
 return array(
     'https' => false,
     'NAME_CONTROLLER' => 'page',    // Default Controller for router
     'NAME_METHOD' => 'index',       // Default Action for router
     'publicWeb' => '',              // Path for public web (web or public_html)
 
     'assets' => array(
         'minifyCssEnabled' => true,
         'minifyJsEnabled' => true,
         'assetsDir' => 'assets',
         'assetsPath' => APP_DIR.'View/',
         'cacheDir' => 'cache',
         'cachePath' => APP_DIR.'../web/',
         'cacheUrl' => HTTP_HOST.'/',
     ),
 
     'routes' => array(
         'docs/:pageId' => array(
             'docs/[pageId]/', 
             'task=page&action=[docsId]&type=docs'
         ),
         
         'error/:code' => array(
             'error/[code]/', 
             'task=page&action=error&type=[code]',
             'args' => array(
                 'code' => '[code]'
             )
         ),
         
         'default' => array(
             '[task]/[action]/[params]',
             'task=[task]&action=[action]',
             'params' => '(.*)',
             '_params' => array(
                 '[name]/[value]/', 
                 '[name]=[value]'
             )
         )
    )   
 
 );

Controller
---------

 - makeUrl - is used for generating the full adress. For example |makeurl| - method used for redirections, equivalent of |header| but with a parameter being a key from the Config/router.php table. In case of using docs/:docsld it looks as the following |redirect|

**Controller/Page.php**

.. code-block:: php

 namespace Controller;
 use Dframe\Controller;
 
 class PageController extends Controller
 {
     public function index(){
         echo $this->router->makeUrl('docs/:docsId?docsId=23');
         return;
     }
 
     public function docs() {
         if(!isset($_GET['docsId'])){
             return $this->router->redirect('error/:code?code=404');
         }
 
     public function error($status = '404'){
         $routerCodes = $this->router->response();
 
         if(!array_key_exists($status, $routerCodes::$code)){
             return $this->router->redirect('error/:code?code=500');
         }
 
         $view = $this->loadView('index');
         $smartyConfig = Config::load('view/smarty');
 
         $patchController = $smartyConfig->get('setTemplateDir', APP_DIR.'View/templates').'/ errors/'.htmlspecialchars($status).$smartyConfig->get('fileExtension', '.html.php');
 
         if(!file_exists($patchController)){
             return $this->router->redirect('error/:code?code=404');
         }
 
         $view->assign('error', $routerCodes::$code[$status]);
         $view->render('errors/'.htmlspecialchars($status));
     }
 
    }
 }

.. |router| cCode:: 
 <?php $this->router; ?>
.. |page/index| cCode:: 
 <?php $this->router->makeUrl('page/index'); ?>
.. |$router| cCode:: {$router}
.. |$makeurl| cCode:: {$router->makeUrl('index/page')}


View
-----

assign - it's a method of the template engine that assignes value to a variable which is used in the template files.

**View/templates/index.html.php**

.. customLi:: myTabs
 :php: active/php
 :smarty: smarty

  .. code-block:: php

   <?php include "header.html.php" ?>
   Example site created using the Dframe Framework

   Routing:
   <?php $this->router->makeurl('page/index'); ?> index/page
   <?php $this->makeurl('error/404'); ?> page/404

   <?php $this->domain('https://examplephp.com')->makeurl('error/404'); ?> page/404

   <?php include "footer.html.php" ?>
   Using only PHP

  - |router| all already available methods used like in |page/index|

  next

  .. code-block:: php

   {include file="header.html.php"}
   Example site created using the Dframe Framework

   Routing:
   {$router->makeurl('page/index')} index page
   {$router->makeurl('error/404')} page 404

   {$router->domain('https://examplephp.com')->makeurl('error/404')} page 404

   {include file="footer.html.php"}
   S.M.A.R.T.Y Engine used in the example

  - |$router| all already available methods are used like in |$makeurl|

**View/index.php**

.. code-block:: php

 namespace View;
 use Dframe\Asset\Assetic;
 
 
 class IndexView extends \View\View
 {
     public function init(){
         $this->router->assetic = new Assetic();
         $this->assign('router', $this->router);

         /* ... */

.. center::

 Dframe\Router\Response

Extention of the basic DframeRouter is DframeRouterResponse, adding functionality of setting the response status (404, 500, etc.) and their headers.

.. code-block:: php

 return Response::create('Hello Word!')
        ->status(200)
        ->headers([
            'Expires' => 'Mon, 26 Jul 1997 05:00:00 GMT',
            'Cache-Control' => 'no-cache',
            'Pragma', 'no-cache'
        ]);

For generating html.

.. code-block:: php

 return Response::renderJSON(array('return' => '1')); 

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

.. |makeurl| cCode:: $this->router->makeUrl('docs/:docsId?docsId=23');
.. |header| cCode:: Header('Location: ""');
.. |redirect| cCode:: $this->router->redirect('page/index');
