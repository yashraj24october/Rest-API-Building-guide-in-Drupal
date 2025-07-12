# Rest-API-Building-guide-in-Drupal
Complete guide to create get and post api in drupal using custom module.

<h3>Pre-requisites</h3>
First you need to install a drupal site then need to create a custom module for all rest api tasks.

<h2>Building GET Request API </h2>
<h4>Steps:</h4>
In your custom module,<br>
<ol>
<li>
Create a <b>Controller</b> folder in which create a file with any name like 'GetRequestController.php'</li>  
  <li> In your GetRequestController.php, add namespace for your controller and then add controller class and extend ControllerBase. <br/>
  <b>Code: </b> <br/>
  <code>
<?php

namespace Drupal\<module_name>\Controller;
use Drupal\Core\Controller\ControllerBase;
<br>
class GetRequestController extends ControllerBase {
    
}

  </code>
  </li>
  <li>In your controller class, create a function for returning your json response. eg: getData(). then import JsonResponse class from <b>Symfony\Component\HttpFoundation\</b>,<br/>
  Using JsonResponse class's object , we can return our api response in json format that can be consumed from anywhere.<br/>
  <code>
    use Symfony\Component\HttpFoundation\JsonResponse; //add above of class
    use Symfony\Component\HttpFoundation\Request; //add above of class
    <br>
     public function getData(Request $request): JsonResponse  {
     $data = NULL;
      // load your data to return here in $data <br>
    <br/>
    return new JsonResponse([
      'status' => 'success/error',
      'received_data' => $data,
    ],200/201/400/403/404);
    <br/>
  }
<br/>
  </code>
<br/>
  By using conditional statement, you can check authentication and send response accordingly.
  </li>
      <li>
        Now, create route for your GET request controller in <b>yourmodule.routing.yml</b><br/>
        In routing.yml file, add your api route like this: <br/>
        <code>
module_name.get_request_api_route:
  path: '/api/get-data'
  defaults:
    _controller: '\Drupal\module_name\Controller\GetRequestController::getData'
  methods: [GET]  
  requirements:
    _permission: 'access content'
        </code>
      </li>
</ol>
<h4>Now done, you are ready to test your api by performing api request using fetch/axios/request etc or also test on postman/thunderclient</h4>
<br/><br/>
<h2>Building POST Request API </h2>
<h4>Steps:</h4>
In your custom module,<br>
<ol>
<li>
For building POST request api same way, Create a <b>Controller</b> folder in which create a file with any name like 'PostRequestController.php'</li>  
  <li> In your PostRequestController.php, add namespace for your controller and then add controller class and extend ControllerBase. <br/>
  <b>Code: </b> <br/>
  <code>
<?php

namespace Drupal\<module_name>\Controller;
use Drupal\Core\Controller\ControllerBase;
<br>
class PostRequestController extends ControllerBase {
    
}

  </code>
  </li>
  <li>In your controller class, create a function for getting posted data and return json response. eg: postData(). then import JsonResponse class and Request class from <b> Symfony\Component\HttpFoundation\</b><br/>
  <h4>Using Request class's object we can get query parameter from route url and also when anyone pass data in request body using post request.</h4>
    <h4>In drupal, Request object has getContent(), that return data passed from post request in string format. so to consume it we can convert it into json/assoc_array(in drupal) using json_decode().</h4>
    <h4> For getting data from query parameter, we can use query->get() method. Request object has query object with method get in which we pass key of data to get data.</h4>
    <br/>
  <code>
    use Symfony\Component\HttpFoundation\JsonResponse; //add above of class
    use Symfony\Component\HttpFoundation\Request; //add above of class
    <br>
     public function postData(Request $request): JsonResponse  {
     $data = json_decode($request->getContent());
    <br/>
    return new JsonResponse([
      'status' => 'success/error',
      'received_data' => $data,
    ],200/201/400/403/404);
    <br/>
  }
<br/>
  </code>
<br/>
  By using conditional statement, you can check data types or keys before returning success response and send response accordingly.
  </li>
      <li>
        Now, create route for your POST request controller in <b>yourmodule.routing.yml</b><br/>
        In routing.yml file, add your api route like this: <br/>
        <code>
module_name.post_request_api_route:
  path: '/api/get-data'
  defaults:
    _controller: '\Drupal\module_name\Controller\PostRequestController::postData'
    _format: 'json'
  methods: [POST]  
  requirements:
    _permission: 'access content'
        </code>
      </li>
</ol>
<h4>Now done, you are ready to test your post api by performing api request using fetch/axios/request etc or also test on postman/thunderclient</h4>


<h3>In this way, using custom module and controller, we can create GET and POST api easily...</h2>


