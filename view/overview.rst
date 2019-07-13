.. title:: View - dframeframework.com

.. meta::
    :description: View - dframeframework.com
    :keywords: dframe, view, smarty, view engine, response, router, dframeframework
    
    
The View folder is used to store the view logic and private items such as engine templates, cache, logs, and uploads.
You can declare or define functions there which can then be called in the view template.


How to load view?
===========

In folder "View" is Index.php it's type of view. You can create difrent views for user or admin. Just copy "Index.php" and rename to your own name.

.. code-block:: php

    /** @var \View\IndexView $view */
    $view = $this->loadView('Index');
    
To use view after load you must select template html.php by

.. code-block:: php

    $view->render('path/to/template/myTemplate'); // myTemplate shoud be in path View/template/path/to/template/myTemplate.html.php
    
    
**View/Index.php**

.. code-block:: php

 class IndexView extends \View\View
 {
     public function init()
     {
         if (isset($this->router)) {
             $this->assign('router', $this->router);
         }
