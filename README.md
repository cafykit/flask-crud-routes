## flask-crud
Flask CRUD Routes to create simple consistent routes. 

## Example 
A very simple example of how to generate routes by using a standard class definition template. 

### The code to do it all. 

     from flask import Flask
     app = Flask(__name__)
     from flask_crud_routes.router import Router
    
     from app_testplans import AppTestplans
     from app_testcases import AppTestcases
    
     with Router(app, AppTestplans):
         with Router(app,AppTestcases):
             pass
             
### We get the following routes defined automatically
| Route | Verbs | Endpoint
|--|--|--|
| /testplans  | HEAD, GET | AppTestplans.index
| /testplans  | POST | AppTestplans.create
| /testplans/<testplan_id>  | HEAD, GET | AppTestplans.show
| /testplans/<testplan_id>  | PUT, PATCH | AppTestplans.update
| /testplans/<testplan_id>  | DELETE | AppTestplans.delete
| /testplans/custom  | POST | AppTestplans.custom
| /testplans/<testplan_id>/testcases  | HEAD, GET | AppTestcases.index
| /testplans/<testplan_id>/testcases  | POST | AppTestcases.create
| /testplans/<testplan_id>/testcases/<testcase_id>  | HEAD, GET | AppTestcases.show
| /testplans/<testplan_id>/testcases/<testcase_id>  | PUT, PATCH | AppTestcases.update
| /testplans/<testplan_id>/testcases/<testcase_id>  | DELETE | AppTestcases.delete


## Controller Definitions 
All the controller definitions are inside a python class derived from Router.Controller
The method names are to be followed as above and the routes are automatically created.  


### A simple controller definition with a custom route.     
    class AppTestplans(Router.Controller):
        def update(self, testplan_id):
            return "AppTestplans::update::{id}".format(id=id)

        def show(self, testplan_id):
            return "AppTestplans::show::{id}".format(id=testplan_id)

        # Define a custom route
        @Router.route(methods=["POST"])
        def custom(self, id):
            return "AppTestplans::custom"

### Another way to write the routes. Helpful if using it in nested resources
    class AppTestcases(Router.Controller):
        def index(self, **kwargs): 
            return "AppTestcases::index::{kwargs}".format(kwargs=kwargs)

        def show(self, **kwargs):
            return "AppTestcases::show::{id}::{kwargs}".format(id=kwargs.get('testcase_id'),kwargs=kwargs)

