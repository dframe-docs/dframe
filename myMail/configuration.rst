The myMail library is a simple wrapper, this time for phpmailer. In a simple way, you can set data for connection in config.

.. code-block:: php

 return array(
    'Hosts' => array('primaryHostName.tld', 'backupHostName.tld'),    // Specify main and backup SMTP servers
    'SMTPAuth' => true,    // Enable SMTP authentication
    'Username' => 'Username@mail',    // SMTP username
    'Password' => '',    // SMTP password
    'SMTPSecure' => 'tls',    // Enable TLS encryption, `ssl` also accepted
    'Port' => 587,    // Port

    'setMailTemplateDir' => './View/templates/mail',
    'smartyHtmlExtension' => '.html.php',    // Default '.html.php'
    'smartyTxtExtension' => '.txt.php',    //Default '.txt.php'
    'fileExtension' => '.html.php',    

    'senderName' => PROJECT_NAME,    //Name of default sender
    'senderMail' => 'senderMail@mail'    //Default sender's address
 );
