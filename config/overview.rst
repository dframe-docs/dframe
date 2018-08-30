.. title:: Config - Help you load configuration from file.

.. meta::
    :description: Helps you load the configuration from a file.
    :keywords: dframe, config, loading, loader, dframeframework
    

Helps you load the configuration from a file. One of the solutions that are often useful. Singular files defining the configuration of a givem module. There will be Config/router.php and Config/View/smarty.php in the example version of the application.

.. code-block:: php

 return [
     'setTemplateDir' => APP_DIR . 'View/templates',       // Default './View/templates'
     'setCompileDir' => APP_DIR . 'View/templates_c',      // Default './View/templates_c'
     'addPluginsDir' => '',                                // Default template dir ./Libs/Plugins/smarty
     'debugging' => false,                                 // Default False
     'fileExtension' => '.html.php'                        // Default '.html.php'
 ];

In practise, it's used for many basic thing from data download. In the example application, it's used to download "setTemplateDir". In the other case, where there wouldn't be a seet value, it's supposed to load the path given as the second parameter.

.. code-block:: php

 <?php
 
 namespace Controller;
 
 use Dframe\Config;
 use Dframe\Controller;
 use Dframe\Router\Response;
 
 class PageController extends Controller
 {
     /**
      * Initial function call
      */
     public function init()
     {
 
     }
 
     /**
      * Catch-all method for requests that can't be matched.
      *
      * @param  string $method
      * @param  array  $parameters
      *
      * @return Response
      */
     public function __call($method, $test)
     {
         $smartyConfig = Config::load('view/smarty');
         $view = $this->loadView('Index');
         $patchController = $smartyConfig->get('setTemplateDir', APP_DIR . 'View/templates') . '/page/' . htmlspecialchars($_GET['action']) . $smartyConfig->get('fileExtension', '.html.php');
 
         if (!file_exists($patchController)) {
             return $this->router->redirect('page/index');
         }
 
         return Response::create($view->fetch('page/' . htmlspecialchars($_GET['action'])));
 
     }
 }
 
