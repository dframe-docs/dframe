.. title:: Cron - dframeframework.com

.. meta::
    :description: cron - dframeframework.com
    :keywords: dframe, cron, smarty, cron engine, crobtab, dframeframework
    

Cron
---------

Cron is a service to perform tasks periodically. It allows you to execute a command at a specified time. Sending emails is such an example.

.. code-block:: php

 <?php
 
 use Dframe\Cron\Task;
 use Dframe\Router\Response;
 
 set_time_limit(0);
 ini_set('max_execution_time', 0);
 date_default_timezone_set('Europe/Warsaw');
 
 require_once dirname(__DIR__) . '/../../../vendor/autoload.php';
 require_once dirname(__DIR__) . '/../../../web/config.php';
 
 /**
  * An anonymous Cron class that calls itself
  */
 return (new class() extends Task
 {
 
     /**
      * Init function
      *
      * @return array
      */
     public function init()
     {
         $cron = $this->inLock('mail', [$this->loadModel('Mail/Mail'), 'sendMails'], []);
         if ($cron['return'] == true) {
             $mail = $cron['response'];
             return Response::renderJSON(['code' => 200, 'message' => 'Cron Complete', 'data' => ['mail' => ['data' => $mail['response']]]]);
         }
 
         return Response::renderJSON(['code' => 403, 'message' => 'Cron in Lock'])->status(403);
 
     }
 
 })->init()->display();
 
Methods
---------

lockTime(string $key, $ttl = 3600)
^^^^^^^^^^^^^^

Lock time

.. code-block:: php

 if ($this->lockTime('mail')) {
     return Response::renderJSON(['code' => 403, 'message' => 'Time Lock'])->status(403);
 }

inLock(string $key, object $loadModel, string $method, $args = [], $ttl = 3600)
^^^^^^^^^^^^^^

This method has a built-in method that blocks it until complete.

.. code-block:: php

 $this->inLock('mail', [$this->loadModel('Mail/Mail'), 'sendMails'], []);
