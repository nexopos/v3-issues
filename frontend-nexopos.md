### Frontend Consideration for NexoPOS
NexoPOS doesn't comes with a frontend available. However, you still have the capacity to extend this by adding 
your own frontend module. It could be a public store or any web portal which uses NexoPOS features.

We do use CodeIgniter on the backend so you'll have to handle the routing by checking all the URI segments, in 
order to have match with your desired route.

### Registering a hook for public frontend
All start by registering a hook on your module (action). This could be defined like this on your main module file :

```php
namespace MyModule;

use Tendoo_Module;

class Module extends Tendoo_Module
{
    public function __construct()
    {
        parent::__construct();
        $this->events->add_action( 'load_frontend', [ $this, 'frontend' ]);
    }
    
    public function frontend()
    {
        echo 'This is frontend';
    }
}
```

Obviously, you could load your own module view using the internal view loader like so : 

```php
// ...
$this->load->module_view( 'my-module', 'home' ); // here "my-module" is your module namespace as defined on your config.xml file.
// ...
```

### Route Mapping
The route mapping is quite simple. Your should have a kind of route layer which check allt he URI segment and 
trigger the right controller accordingly. For example :

```php
namespace MyModule;

use Tendoo_Module;
use MyModules/Frontend/Controllers/StoreController;

class Module extends Tendoo_Module
{
    public function __construct()
    {
        parent::__construct();
        $this->events->add_action( 'load_frontend', [ $this, 'frontend' ]);
    }
    
    public function frontend()
    {
        if ( $this->uri->uri_string() === 'home/store' ) {
            return ( new StoreController )->store();
        }
    }
}
```

Within your controller you would organise the header, the footer the asset loading as well.
