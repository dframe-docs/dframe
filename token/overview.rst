.. title:: Token - Library used to create tokens secures you against CSRF

.. meta::
    :description: Library used to create tokens secures you against CSRF (Cross-Site Request Forgery), so against unauthorized executing of action.
    :keywords: dframe, Token, CSRF, tokens, Cross-Site Request Forgery, dframeframework  

Tokens
===========

Library used to create tokens secures you against CSRF (Cross-Site Request Forgery), so against unauthorized executing of action.

Initializing a token

To be able to start using tokens for the Bootstrap.php file, you need to add the following to __construct

.. code-block:: php

 $this->session  = new \Dframe\Session(SESSION_NAME);
 $this->token  = new \Dframe\Token($this->session);

Example usage:

.. code-block:: php

 if (!$this->baseClass->token->isValid('evidenceToken', (isset($_POST['token']) ? $_POST['token'] : null))) {
     return Response::renderJSON(['code' => 403, 'message' => 'The form has expired.'])->status(403);
 }
            
 $evidenceToken = $this->baseClass->token->generate('evidenceToken')->get('evidenceToken');
 
 
Smarty3 Plugin
===========

.. code-block:: php

 <?php
 /*
  * Smarty plugin for Dframe\Token
  * -------------------------------------------------------------
  * File:     function.token.php
  * Type:     function
  * Name:     token
  * Purpose:  outputs a token
  * -------------------------------------------------------------
  */
  
 /*
  * Instalation: 
  * Put file in to app/Libs/Plugins/smarty/function.token.php
  * Usage: {token name='userToken'}
  */
 function smarty_function_token($name){
     $token = new \Dframe\Token(new \Dframe\Session(APP_NAME));
     return $token->generate($name['name'])->get($name['name']);
 }
