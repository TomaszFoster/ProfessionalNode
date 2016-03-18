#### event-driven programming style:

example:
```js
result = query("SELECT * FROM posts WHERE ID=1");
do_something_with(result);
```
this is the typical blocking style of I/O programming. To do the same thing asynchronously without blocking, we could do the following:
```js
query_finished = function(result){
	do_something_with(result);
}

query('SELECT * FROM posts WHERE ID=1', query_finished);
```
In this method we're defining the function and passing it through the query as a callback so that when it's finished running, it will run the function with the results. This method would allow other functions to run simultaneously. 

This method of programming is called event-driven or asynchronous programming.

The benefit of this is that several I/O operations can be running in parallel and each respective callback function is invoked when the operation finishes.

This style also has an event-loop for event detection and event handler triggering in a continuous loop.

There is at most ONE event loop running at any given time.

Reminder, always good practice to wrap functions in an anonymous function to avoid polluting the global namespace, i.e.
```js
(function(){
	do_something = function(){
		alert('this');
	}
	var something_else = 0;
	counter = 2;
}())
```
None of these will be defined in the highest mode global namespacing and thus reduce the likelihood of namespace conflicts.

Line 34 invokes the function immediately after calling it.

