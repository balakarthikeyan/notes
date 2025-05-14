### How to create a controller in Symfony?
A controller in Symfony is used to take information from the HTTP request and returns an HTTP response in the form of a Response object.

```php
// Symfony 4 
use Zend\Mvc\Controller\AbstractActionController;  
use Zend\View\Model\ViewModel;    
class IndexController extends AbstractActionController {  
   public function indexAction() {  
      return new ViewModel();  
   }  
}

// Symfony 7
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Attribute\Route;

final class HelloController extends AbstractController
{
    #[Route('/hello', name: 'app_hello')]
    public function index(): Response
    {
        return $this->render('hello/index.html.twig', [
            'controller_name' => 'HelloController',
        ]);
    }
}
```

### Which method is used to set and get the session in Symfony?
To store user information between requests, Symfony offers a session object and a number of utilities. We can use the method `SessionInterface` to set and get sessions.

- `$session->set()` is used to set an attribute.
- `$session->get()` is used to get an attribute that some controller already sets.

```php
public function sessionAction(SessionInterface $session)  
{  
    // to set an attribute
    $session->set('user_id,' 1000);  
    // to get the attribute that a controller already sets
    $user_id = $session->get('user_id');    
}
```

### What is the syntax for EmailType in Symfony?
The EmailType field in Symfony is a text field. The HTML `<input type="email"/>` tag is used to render it.

```php
use Symfony\Component\Form\Extension\Core\Type\EmailType; 
$builder->add('token', EmailType::class, array('data' => 'hello,'));
```
You can use the type as an array if you want to provide extra attributes to an HTML field. The data field overrides the initial value for all the fields.

### Write the method used to get the Request Parameters?
Symfony provides a Request class which is used to request information. It is an object-oriented representation of the HTTP request message. Use the following method: 

```php
$request->query->get('parameter_name');
```

### What do you mean by form Helper? List the form helper functions in Symfony?
To write form inputs in templates, we use form helpers. They are used mostly for complex elements like drop-down lists, date, and rich text. Some of the helper functions are:

- form_tag()
- textarea_tag()
- select_tag()
- options_for_select()
- checkbox_tag()
- input_tag()
- input_file_tag()
- input_password_tag()
- input_hidden_tag()
- submit_tag()

Form Helper Function 

HTML form tag points to a valid action, route, or URL. 
`{{ form_start(form, {'attr': {'id': 'form_person_edit'}}) }}`

Closes the HTML form tag after using form_start.
`{{ form_end(form) }}`

An XHTML compliant label tag 
Returns with the specified parameter. 

An XHTML compliant input tag with type=“checkbox”.
```php
echo checkbox_tag('choice[]', 1);
echo checkbox_tag('choice[]', 2);
echo checkbox_tag('choice[]', 3);
echo checkbox_tag('choice[]', 4);
```

An XHTML compliant input tag with type = “password”.
```php
echo input_password_tag('password');
echo input_password_tag('password_confirm'); 
```

An XHTML compliant input tag with type = “text”.
`echo input_tag('name');`

An XHTML compliant input tag with type = “radio”. 
```
echo ' Yes '.radiobutton_tag(‘true’, 1);
echo ' No '.radiobutton_tag(‘false’, 0);
```

An XHTML compliant input tag with type = “reset”.
`echo reset_tag('Start Over');`

A select tag populated with all the countries in the world. 
`echo select_tag('url', options_for_select($url_list), array('onChange' =>'Javascript:this.form.submit();'));`

An XHTML compliant textarea tag
Returns a textarea tag wrapped with an inline rich-text JavaScript editor. 

An XHTML input tag with type = “submit”.
`echo submit_tag('Update Next Record'); `

### Describe the method used to handle an Ajax request?

> We use the current request object's `isXmlHttpRequest`() method; if it can handle an Ajax request, it will return true, or else it will return false. 

```php
if ($request->isXmlHttpRequest()) {    
    // Ajax request    
} else {    
    // Normal request    
}
```

### What is the method to create action in the Symfony controller?
In order to serve HTML, you will need to render a template, which can be done using the render() method. It is used to put the content to be rendered in a Response object.

To create an action in the Symfony controller, we need to implement the following method:
```php
public function indexAction()  
{
    return $this->render('user/index.html.twig', [ ]);  
}
```

### What is the syntax to install Symfony 
For Windows:

`php -r "readfile('https://symfony.com/installer');" > symfony`

For Mac and Linux:

```bash
sudo mkdir -p /usr/local/bin    
sudo curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony    
sudo chmod a+x /usr/local/bin/symfony
```

### How to get current Version of Symfony?

To find the version of Symfony, run the following commands in the command prompt 

```bash
$ php bin/console --version 
Symfony 5.1.1 (env: prod, debug: false) 
$ php app/console --version 
Symfony version 2.4.1 - app/dev/debug 
$ php ./symfony --version 
symfony version 1.4.21-punkave (/var/www/p/releases/20200504161617/lib/vendor/symfony/lib) 

## The version can be found within the project files. 

vendor/symfony/http-kernel/Kernel.php 
vendor/symfony/symfony/src/Symfony/Component/HttpKernel/Kernel.php
 
const VERSION = '5.0.4'; 

## Version can also found by using the following code: 
$request = $this->container->get(‘request’); 
$currentRouteName = $request->get(‘_route’); 
```

### How can the request parameters be obtained in Symfony

```php
$request = $this->container->get('request');  
$name = $request->query->get('name')
```

### What is the syntax for checking a valid email address?

```php
use Symfony\Component\Validator\Constraints as Assert; 
class Student {     
    /**   
    * @Assert\Email(   
    * message = "The email '{{ value }}' is not a valid email.",   
    * checkMX = true   
    * )
    */     
    protected $email;
} 
```

### What is the default port of Symfony?
80 is the default port in Symfony.

### How can an action be created in the controller of Symfony 
Action in Symfony ###can be created in the Symfony controller with the help of given commands:
```php
public function indexAction()    
{    
    return $this->render('user/index.html.twig', [ ]);    
}
```

### List the rules you need to follow while creating methods inside the controller.

- The "Action" suffix must be used in Action methods.
- Long controller methods should not be used; refactoring it.
- The public method should be used in only actions method.
- Valid response objects should be used by the action method.

### List some benefits of Symfony.
- Development is fast
- Works on MVC design patterns
- It is expandable
- Better user control 
- User flexibility is unlimited 
- It is a feasible and stable framework

### How can we protect a route with authentication in symfony

We have several ways to protect access to our routes, the first is in the `security.yaml` file and the second is using the method `$this->denyAccessUnlessGranted('ROLE_ADMIN');` in the method that we want to protect.

### What are the methods associated with a resource type controller in symfony

The index method uses the get method by default, and we will use it to obtain a data 

```php
#[Route('/', name: 'user_index', methods: ['GET'])] 
public function index(UserRepository $userRepository): Response 
{ 
//Code should be written within this function 
} 

// The new method uses the post method, and we use it to create data 
#[Route('/new', name: 'user_new', methods: ['POST'])] 
public function new(Request $request, EntityManagerInterface $entityManager): Response 
{ 
//Code should be written within this function 
} 

// The show method uses the get method, and we will use it to obtain data through a parameter. 
#[Route('/{id}', name: 'user_show', methods: ['GET'])] 
public function show(int $id): Response 
{ 
//Code should be written within this function 
}

// The edit method uses the put or patch method, and we will use it to update a data 
#[Route('/{id}/edit', name: 'user_edit', methods: ['PUT', 'PATCH'])] 
public function edit(Request $request, User $user, EntityManagerInterface $entityManager): Response 
{ 
//Code should be written within this function 
}

// The delete method uses the delete method, and we will use it to delete data 
#[Route('/{id}', name: 'user_delete', methods: ['Delete'])] 
public function delete(User $user): Response 
{ 
//Code should be written within this function 
} 
 ```

### How to get the current route in the Symfony framework?

_route was always meant for debugging.

```php
$router = $this->get("router"); 
$route = $router->match($this->getRequest()->getPathInfo()); 
var_dump($route['_route']); 
```

### Differences in annotations between versions 5 and 6 in symfony?

```php
// In symfony version 5 paths use the sign @ 
@Route("/", name="user_index", methods={"GET"}) 

// In symfony version 6 paths use the sign # 
#[Route('/', name: 'user_index', methods: ['GET'])] 
```
