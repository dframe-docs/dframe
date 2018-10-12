.. title:: Session - Store information about the user

.. meta::
    :description: This is a simple framework element, allowing for quick session operations in php.
    :keywords: dframe, Session, fingerprint, store, information, dframeframework   


Description
===========

This is a simple framework element, allowing for quick session operations in php. One-line defining of a global variable.

Instead of checking whether it exists in one line, we receive those two things.

|get|

In this case, we download |sesok| and if it doesn't exist right away, we define it with the second parameter |false|. You can put any value in place of |false|.

Assigning to |baseclass| is found in the app/bootstrap.php file, as |name|. Parameter is relayed to the object and defines the session name.

=======
Standalone
=======

Simplicity and minimalism make you want to use the same class. In order to achieve that, the following methods are available in the project.

.. code-block:: php
  
 $this->session = new \Dframe\Session('name');
 $session = new Session('HashSaltRandomForSession');
 $session->register(); // Set session_id and session_time - default 60
 $session->authLogin(); // Return true/false if session is registrer
 $session->set($key, $value); // set $_SESSION[$key] = $value;
 $session->get($key, $or = null); // get $_SESSION[$key];
 $session->remove($key) // unset($_SESSION[$key]);
 $session->end(); // session_destroy

.. |get| cCode:: $this->baseClass->session->get('ok', false); 
.. |sesok| cCode:: $_SESSION['ok']
.. |baseclass| cCode:: $this->baseClass->session
.. |false| cCode:: false
.. |name| cCode:: $this->session = new \Dframe\Session('name');

.. |newSession| cCode:: $session = new Session('HashSaltRandomForSession'); 
.. |register| cCode:: $session->register(); // Set session_id and session_time - default 60 
.. |authLogin| cCode:: $session->authLogin(); // Return true/false if session is registrer 
.. |set| cCode:: $session->set($key, $value); // set $_SESSION[$key] = $value; 
.. |sGet| cCode:: $session->get($key, $or = null); // get $_SESSION[$key]; 
.. |remove| cCode:: $session->remove($key) // unset($_SESSION[$key]);
.. |end| cCode:: $session->end(); // session_destroy
