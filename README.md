# This plugin is for Drupal 8.x
The plugin  is developed to made a simple ajax call from your theme or module and get response, similar like in wordpress
## Install this module on your drupal folder "modules"
## After activating the module Clear cache from admin panel, from here http://yourwebsitename.com/admin/config/development/performance
 
##### 1) Create a file in your theme or module call it functions.php 
##### 2) In this file functions.php create any method with any name for example let's said 

###### Example php
 
}

```php
class Functions{

public function get_data_from_node() {
// Get post data
$limit = \Drupal::request()->request->get("limit"); // OR $limit = $_POST['limit'];
$id = \Drupal::request()->request->get("id"); // OR $limit = $_POST['id'];

  // make a simple mysql query, could be insert or update or extract
   $nids = \Drupal::entityQuery('node')
   ->condition('type', 'article')
   ->condition('nid', $id, '<')
   ->sort('nid', 'DESC')
   ->range(0, $limit)
   ->execute();
 $nodes = \Drupal::entityTypeManager()
   ->getStorage('node')
   ->loadMultiple($nids);
      

      foreach( $nodes as $val){
             echo "<p>". $val->body->value."</p>"  ; // here will show the article content
        }
} //end method get_data_from_node

} //end class Functions

```

##### 3) HTML CODE, this code is from your *.html.twig
 
```html
<a class="btn" href="#" onclick="my_ajax_load(); return false;">Ajax Test</a>

<div id="ajax-target">Ajax goes here!!!</div>
```

##### 4) jQuerey CODE

```javascript 
function my_ajax_load() {
  var str = "id=22&message=where id= '?'";
  $.post("/callajax", {'action':'get_data_from_node','id':'6', 'limit':'5' }, function(data) {
      $("#ajax-target").html(data);
   });
}

 ```
 
##### URL TO SEND POST DATA ALL THE TIME IS THIS  http://mywebsite.com/callajax 
  
###### where:
  
- "/callajax" - is url // please don't change, keep how is it **/callajax**
- 'action' - is the name of your function in our case is "get_data_from_node" you can rename how you like
- 'id' - simple post var
- 'limit' - simple post var

#### if you want to make ajax call from your module 

##### 1) add to post var type where **type** is the name of module
example javascript for module 

##### How will look the jQuerey CODE
 ```javascript
function my_ajax_load() {
  var str = "id=22&message=where id= '?'";
  $.post("/callajax", {'action':'get_data_from_node','type':'MyModuleName','id':'6', 'limit':'5' }, function(data) {
      $("#ajax-target").html(data);
   });
}
 
```

###### where
 
- 'type' - is a module name, your module name
 


##### That's it! Simply An Easy
