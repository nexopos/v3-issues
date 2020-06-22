So you're interested by creating applications working with NexoPOS through the Rest API ? don't look further, we're about to let you know 
the basic of creating a Rest API connexion with any NexoPOS installation. Let's get started.

## Authentication of the request
Since, apps which connect to NexoPOS aren't registered within as reliable application, we do assume all request are reliable. The user, should
the be sure that the application requesting the access is reliable and can be trusted. Having that said, let's see how to create a oauth request.

Unlike usual Oauth Authorization page, NexoPOS offer a slightly different approach. You need to provide as GET parameters : 
- a callback as "cb" ie : "?cb=http://yoursystem.com".
- a scopes parameters as "scopes" ie: "?scope=scope1,scope2,scopes3"

So the request shouldbe made to the system where NexoPOS is installed using the following address (as an example).
`https://yourwebsite.com/oauth?cb=http://yourremoveapp.com&scope=core`

If the user approves the request, you'll be redirected to the callback provided with the token required to perform API request to NexoPOS. Usually the callback URL will look like this: 

`http://yourremoteapp.com/?key=[randomkey]&request_header=[request_header]`

The request header should be available on each request sent to NexoPOS. And the value fo that header should be the [randomkey] provided by the URL.

```
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36
X-API-KEY: hjGCLfjlfhsfsBGhIF5SQt2147NxgBFaR5VCMrVk
X-Requested-With: XMLHttpRequest
```
In the preview example, "X-API-KEY" is the [request_header] and the code provided as a value, is the [randomkey]. Request not having these informations will always fails using the API Ressource.

## How to register a scope
If you have created a module within NexoPOS, you can define your own scope and register it from the OauthLibrary embeded on NexoPOS.

```php
// assuming we're on a class
$this->load->library( 'OauthLibrary' );
$configuration  = array(
    'label'            =>        __('Access user details'),
    'description'    =>        __('The current request would like to access user informations such as name, email, except password'),
    'app'            =>        'profile',
    'icon'            =>        'fa fa-user',
    'color'            =>        'bg-aqua',
    'group'            =>        array( 'user', 'master', 'administrator' )
);
$this->OauthLibrary->addScope( 'namespace', $configuration );
```

let assume you're willing to get an access to a scope with the following namespace : "check_sales", then your final URL should be like so :

 `http://yournexoposinstallation.com/oauth?cb=http://yourcallback.com&scopes=check_sales`
 
 ## How to use existing scopes
 NexoPOS comes with some basic scopes : "core", "profile", defined here: application\config\oauth.php
 
 ## What about the authentication token
 Once the user authorize the request, NexoPOS will redirect back to your callback with the token embeded on the GET variable : key & key_header
 
 The key is the value of the header variable "key_header" which should be send along on each of your incoming request (incoming on NexoPOS). So, the key_header might be something like "X-API-KEY" and the "key" will be a hashed value that should be send on your request : 
 
```
"key_header" = "key"

"X-API-KEY" = "[random code]"
```

## How to use the existing route endpoints
Most of NexoPOS operation goes through the API endpoint. This works for Gastro as well. Both save their API endpoint on the following files : 
- application\modules\gastro\api.php
- application\modules\nexo\api.php

This is a Laravel route like configuration. And the API controller methods are saved under the "api" folder of each module folder.
- application\modules\nexo\api
- application\modules\gastro\api


