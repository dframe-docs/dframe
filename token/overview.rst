Tokens
----------

Library used to create tokens secures you against CSRF (Cross-Site Request Forgery), so against unauthorized executing of action.

Initializing a token

To be able to start using tokens for the Bootstrap.php file, you need to add the following to __construct

.. code-block:: php

 $this->session  = new \Dframe\Session(SESSION_NAME);
 $this->token  = new \Dframe\Token($this->session);

Example usage:

.. code-block:: php

 if (!$this->baseClass->token->isValid('evidenceToken', (isset($_POST['token']) ? $_POST['token'] : null))) {
     return Response::renderJSON(array('return' => '0', 'response' => 'Formularz wygasł.'));
 }
            
 $evidenceToken = $this->baseClass->token->generate('evidenceToken')->getToken('evidenceToken');
 
