# Web Application Framework
A tiny PHP web application framework.

I'm mostly doing this as an exercize in picking through some decisions other frameworks have made.

## Keeping it simple
I think that everyone has their own idea of simple, this is designed for a project that you want to be able to easily test with phpunit and dependency injection but isn't so large that you need to split your project into multiple packages within the project.  The idea is that if your project becomes that big you should probably use something like service oriented archetecture to split it up anyway.

## Design goals
* MVC style framework.
* Di/Configuration autocompletes in PhpStorm 10+
* PSR 2 compliant
* Di/Configuration both work the same way, and can be configured per environment.
* Possibly no external libraries for the framework itself though I will be giving installation instructions with composer.
* phpunit will be used in examples to run tests.
* Easy to debug from end to end. I want to hit very little code before hitting the custom logic in the controller.
* A handful of common simple objects that you can optionally use (Session, Log, Request, Response)
* To implement the example project I am building I will look into using the controller with REST verbs, though I'm currently unsure of how I plan to implement it.

## Example flow when using this framework
An http POST happens: http://yoursite/Account/save

Apache rewrite sends everything to http://yoursite/index.php behind the scenes with a path of /Account/save

index.php calls the routing code that loads the controller Account and calls the method saveAction.

The controller then makes sure the person is logged in and is who they say they are.

The controller then does data validaton to make sure everything being passed in is sane.

The controller then asks the dependency injection container for the AccountManager.  The dependency injector makes another call to create an AccountRepository that then requries and so creates a PDO connection to the database as a singleton. The AccountManager is returned with everything it needs.

The controller then uses the AccountManager to save the object.

The controller then returns a json encoded response indicating everything went well.

## Changes to above flow when using REST
An http PUT happens to : http://yoursite/Account/12345

The router sees that rest is enabled on the account controller and calls the account/put method.

The put method then ensures that the person is logged in, validates the data, gets the repository and updates the object in the repository, and returns an affermative response.

## Installation With Composer
Since composer is an easy way to pull in additional libraries you will need for your project I'm going to just explain how to install this project with composer.

## Unit Testing
In general you just put tests in the testing directory with the same structure as the classes you are testing in the classes directory.  The environment is Testing and so the associated configuration and dependency injection classes will be loaded.
