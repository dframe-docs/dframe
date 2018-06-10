.. title:: View - dframeframework.com

.. meta::
    :description: View - dframeframework.com
    :keywords: dframe, view, smarty, view engine, response, router, dframeframework
    
    
The View folder is used to store the view logic and private items such as engine templates, cache, logs, and uploads.
You can declare or define functions there which can then be called in the view template.

.. code-block:: php

 class IndexView extends \View\View
 {
     public function init()
     {
         if (isset($this->router)) {
             $this->assign('router', $this->router);
         }
