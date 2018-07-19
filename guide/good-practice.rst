.. meta::
   :description: Good Practice - Dframe Framework
   :keywords: guide, tutorial, Good Practice, dframe framework, controller, model, php, php 7

Good Practice
===========

Goood practice allow keep your code in harmony. Writing code do not tell youtself that he is only for you. Will come once a time that you will need help somone. So if your code will be unreadable it will take more time to analyze what's the problem is.

 - Before loading View load first Model. 
 - Name of the variable create from name model and word Model ($NameModel) 
 - Variable view we use usually once so create just $view 
 

.. code-block:: php

 public function myMethod()
 {
     $FirstModel = $this->loadModel('First');
     $SecondModel = $this->loadModel('Second');
     $View = $this->loadView('Index');
     
     /* ... */
 }

If you have some validation access code try to do like this

.. code-block:: php

 public function myProcetedMethod()
 {
     if ($this->baseClass->session->authLogin() != true) {
         return $this->baseClass->msg->add('s', 'Unauthorized.', 'page/index');
     }
 
     $FirstModel = $this->loadModel('First');
     $SecondModel = $this->loadModel('Second');
     $View = $this->loadView('Index');
     
     /* ... */
 }


If you are using JSON try like this

.. code-block:: php

 public function myProcetedAndPostMethod()
 {
     if ($this->baseClass->session->authLogin() != true) {
         return Response::renderJSON(['code' => '401', 'message' => 'Unauthorized'])->status(401);
     }

     $method = $_SERVER['REQUEST_METHOD'];
     switch ($method) {
     
        case 'GET':
           /* ... */
           break;
           
        case 'POST'
        
           if (!isset($_POST['someValue']) AND !empty($_POST['someValue'])) {
              return Response::renderJSON(['code' => '400', 'message' => 'Bad Request', 'errors' => ['Invalid Values']]))->status(400);
           }

           $FirstModel = $this->loadModel('First');
           $SecondModel = $this->loadModel('Second');
           
           /* ... */
           
           return Response::renderJSON(['code' => '200', 'message' => '', 'data' => []]);
           break;
           
     }
     
     return Response::renderJSON(['code' => '405', 'message' => 'Method Not Allowed'])->status(405);
     
     //...
 }
