# a-better-angular-app
Let’s make our angular application a better application.

Before starting I must mention, Angular is the best technology happen to me after mighty Java. 
Weird? A back-end engineer talking about front-end!! Yeah that’s the impression it had on me. 
I just enjoy working with it. It makes life so easy with SPA and two way binding.

###1. Try creating your directory structure based on features within your application.

Folder Structure
![alt text](https://github.com/Nasruddin/a-better-angular-app/blob/master/folder-structure.png "Folder Structure")

Above structure can help you accessing your component in a better way and will make your life much easier as the project component grows.

###2. Making Modules Local as opposed to making it global. 

```
var loginModule = angular.module('loginModule', []);
    loginModule.controller('firstAppController', function($scope){

});
```

In the above example as we are storing the module in a variable, loginModule, the Javascript will it a global scope. And it’s always recommended to avoid making a global scope which can be overridden any bad day and causing a abrupt issues in production.

To overcome the above flaw we can use the below snippet that angular has given.

```
angular.module('firstApp', []);
angular.module('firstApp').controller('firstAppController', function($scope){

});
```

So when you call angular.module with two agruement, then angular makes a new module, where as when you call the angular with single arguement then angular gives you back the existing module.

###3. Clashes of the Titans One way binding VS Two way binding. 

Angular comes with the magical two binding out of the box which means when view changes value then model gets notified and undergoes change through dirty checking, and if models changes the value the view automaitcally get updated. Isn’t it great.

But it comes with a cost. All variable which can be changed in its lifecycle comes with a watcher which it watching its behavior and changes it undergoing. Watchers are expensive and can give a performance issue in long term.

Example :
```
<div ng-controller="nameController">
  <input type="text" ng-model="name">
  <h3>{{name}}</h3>
</div>
```
So to overcome this issue we can use single way binding which comes with angular 1.3

```
<h3>{{::name}}</h3>
```
So when you want to update a value one-way which follows top-down approach, use one way binding. However when you want update a value which is changing within a input element that has a ng-model directive applied to it, use two way binding.

It’s recommended that the number of shouldn’t exceed more than 2000 bi-directional data binding on the page for each $Digest cycle.

###4. Optimizing ng-repeat 

```
<ul>
  <li ng-repeat=”name in names”>
    <h3>{{name.firstName}}</h3>
    <h3>{{name.lastName}}</h3>
  </li>
</ul>
```

ng-repeat is the coolest feature of angular and I use it a lot in my applications. But the bad
part is ng-repeat the comes with delays and performance issue.

Lets analyze the above example.
Can you can guess how many watchers are we going to have to a single element in the list?
Answer is 2+1 = 3

Angular will create one extra watcher for ng-repeat directive. Suppose we have 100 of items in the names then watchers will touch the figure of 300.

Always use ng-repeat with track by clause. It will help in reusing your existing DOM element rather than creating them again. This helps when you have pagination in your page.

###5. $scope : The real love between Controller and View 

$scope binds the UI with controller. The most important element that Angular brings on the table.
But  the $scope is very expensive object, as it is the one which goes dirty check. Just bind the variable on the scope which you want to show on the UI and rest variables should stay in controller not on the scope.

###6. The game of speed : $scope.disgest() versus $scope.apply() 

$digest loop and $apply loop comes into play when we are using any other javascript library which doesn’t the feature of data binding. And we want to get the two data binding get going within our angular application’s module which is a using a javascript library. Both digest and apply loop help in creating the watchers for them so that they perform as expected.

$scope.$apply() is very crictal feature of angular which has to be use very judiciously. $disgest loop checks the particular $scope to which it is associated as well as it’s precedent children. Where as when we call $apply it’s check your $rootScope and it’s all children. So it’s always good to call $scope.$digest() loop.

###7. Using custom $watch() function carefully 

A $watch function register watcher to moniter the model changes.

Within a $digest loop $watch functions will be called for $scope variables. So it’s always recommended to keep the $watch function clean with less business logic, loops and deep comparisons.

```
$scope.$watch('someModel',function(newValue,oldValue){
  if(newValue!=oldValue){
  //do something
  }
}, true);
```
In the above snippet we are passing True as third parameter which will do a deep comparison, which checks every property with angular.equals() function.
And the angular.equals() function makes a copy of the object, and stores and saves it while it needs to walk through every property to check if any of them have changed it. It affects the performance terribly.

The return value of $watch function can used to unbind the watcher, which will clear the memory allocated to watcher.

```
var unbind  = $scope.$watch('someModel',function(newValue,oldValue){
  if(newValue!=oldValue){
  //do something
  unbind();
  }
});
```

###8. DOM manipulation. 

Developer : Controller can you do DOM manipulation for me?
Controller:  Common don’t give me so much of work load. Why don’t you try asking directives.
Developer: Directive can please help me out with this?
Directive: Yeah buddy, you know what I do wonders with jqLite.

It’s always good to keep your DOM manipulation away from controllers. Don’t write your DOM manipulation logic inside your controller. The best practice is to keep your DOM manipulation inside your directives with the help of jqLite.

Say No to jQuery, Why? Because :
  1. It is not reusable.
  2. It is not testable.
  3. It include css hardcoded selectors dependencies.

 
###9. Use promise for interacting to REST apis 

A promise object represents a value that may not be available yet, but will be at some point in future. It enables you to write asynchronous code in a more synchron- ous way.
AngularJS enables you to create and use promises through a built-in service called $q .

```
Promise.then(function(response){
//Handle your response
},
function(error){
//Handle the error
},
function(progress){
//Show the progress
})
```

So significant aspect to note here is that the then function returns the promise itself, which means that you can actually chain promises, to create concise blocks of logic that are executed at the appropriate times, without lots of nesting. That gives lots of readability.

```
Promise.then(
function(response){
//First response
})
.then(
function(response){
//Second response
})
```

###11. Avoid using ng-with custom directives. 
 
###12. Use ng-cloak to showing expression {{}} till your angular is loaded. 

###13. When to use ng-if and ng-show 

  1. ng-if will remove element from the DOM, which means that all your handlers or anything else attached to those elements will be lost. So when the element is remove, the $scope gets destroyed and creates a new $scope is created when we restored the element. 

  2. ng-show / ng-hide will hide the elements rather than removing it. CSS class is used to hide and show the element within the DOM tree.

###14. Service should have logic which is  independent of view 

###15. Lets talk : Controller to Controller 

If you want your to communicate together think of services, which is singleton. Or fire an event on the $scope, and have the other controller to watch. Apart from this don’t use any other way to communicate between two controllers.

###16. Tools I use : 

#####Webstrom : The support for angular in webstorm makes your life easy.
#####Batarang : Chrome extension to debug angular.
