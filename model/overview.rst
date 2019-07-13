.. title:: Model - Create methods that have database queries

.. meta::
    :description: In the model, you can create methods that have database queries and are responsible for data processing.
    :keywords: dframe, model, mysql, database, dframeframework  
    

Description
===========

In the model, you can create methods that have database queries and are responsible for data processing. You have to remember to install dframe/database package before starting work. It's available on |github| GITHUB or through the composer.

#How to load model?

In folder "Model" you must model named "RequestModel" should look like *Models/RequestModel.php*

.. code-block:: php

    /** @var \Model\RequestModel $requestModel */
    $requestModel = $this->loadModel('RequestModel');

**Models/RequestModel.php**

.. code-block:: php

    namespace Model;

    class RequestModel extends \Model\Model
    {

        /**
         * @param int $requestId
         * @return mixed
         */
        public function getRequestSettings($requestId)
        {
            $row = $this->pdoQuery('SELECT * FROM `request_type` WHERE request_type_id = ?', [$requestId])->result();
            return $this->methodResult(true, $row);
        }
    }


Notice that virtually all methods, except for a few, return the data in the form of a ready to read table and are returned by the method.

.. code-block:: php

 /**
   * @param boolean
   * @param array
   */
 $this->methodResult(true, $row);

.. |github| fa-icon:: fa-github
