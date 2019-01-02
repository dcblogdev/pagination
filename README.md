pagination
==========

Paginate record sets, not tied in directly to a database.

## Usage
1. include the class
2. instantiate a new object pass in the number of items per page and the instance identifier, this is used for the GET parameter such as ?p=2
3. pass the set_total method the total number of records
4. show the records 
5. call the page_links method to create the navigation links

 ````php
 include('paginator.php');
 
 $pages = new Paginator('10','p');
 $pages->set_total('100'); //or a number of records
 
 //display the records here
 
 echo $pages->page_links();
 ````
 if using a database you limit the records by placing $pages->get_limit() in your query, this will limit the number of records
 
 ````php
 select * from table $pages->get_limit()
 ````
 
by default the page_links method created links starting with ? this can be changed by passing in a parameter to the method:

````php
echo $pages->page_links('&');
````

The method also allows you to pass in extra data such as a series of GET's

````php
echo $pages->page_links('?','&status='.$_GET['status'].'&active='.$_GET['active']);
```` 
 
## Database example

````php
//include the class
include('paginator.php');

//create new object pass in number of pages and identifier
$pages = new Paginator('10','p');

//get number of total records
$stmt = $db->query('SELECT id FROM table');
$total = $stmt->rowCount();

//pass number of records to
$pages->set_total($total); 

$data = $db->query('SELECT * FROM table '.$pages->get_limit());
foreach($data as $row) {
    //display the records here
}

//create the page links
echo $pages->page_links();
````

## MVC example

using this class in an MVC environment its almost the same, only the database or dataset calls come from the model instead of the page directly.

in the controller:

````php

//create a new object
$pages = new Paginator('10','p');

//set the total records, calling a method to get the number of records from a model
$pages->set_total( $this->_model->get_all_count() );

//calling a method to get the records with the limit set
$data['records'] = $this->_model->get_all( $pages->get_limit() );

//create the nav menu
$data['page_links'] = $pages->page_links();

//then pass this to the view, may be different depending on the system
$this->_view->render('index', $data);
````
