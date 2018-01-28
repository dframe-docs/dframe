====
Description
====

In the model, you can create methods that have database queries and are responsible for data processing. You have to remember to install dframe/database package before starting work. It's available on |github| GITHUB or through the composer.

.. code-block:: php

 namespace Model;
    
    class RequestModel extends \Model\Model
    {
        /**
         * @parms int 
         * return array(boolean, array)
         */
    
        public functio1n getRequestSettings($requestId){
            $row = $this->baseClass->db->pdoQuery('SELECT * FROM `request_type` WHERE request_type_id = ?', array($requestId))->result();
            return $this->methodResult(true, $row);        
    }

Notice that virtually all methods, except for a few, return the data in the form of a ready to read table and are returned by the method.

.. code-block:: php

 /**
 * @parms boolean
 * @parms array
 */

 $this->methodResult(true, $row);

.. |github| fa-icon:: fa-github
