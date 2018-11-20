---
title: Understanding debounce es6
date: "2018-11-19T00:02"
---
### (And when would you use it)
Debounce, plain and simple, is a way for you to limit the number of times a function is called. A debounce method is a great thing to put in place to increase performance out of your javascript application.

### Okay, so when would you use it?
Let’s say that you have an ajax call that happens when a user types into a search field. Triggering this call every ‘keyup’ event can get pretty taxing, and a bit unnecessary.  This is where debounce comes in.

### Debounce
Here is a debounce method implemented using ES6

```
// timeout is our timer containing our later method call.
// later() is called only if the timeout times out, and
// if it's not an immediate call.
// .apply( context, args) let's us call the function with 'context' i.e. "this"
const debounce = (func, wait, immediate ) => {
	let timeout;

	// Later is only called if the timeout times out.
	const later = () => {
		timeout = null;
		if ( !immediate ) return func.apply( this, arguments);
	}

	// Create the setItemout passing in the later function.
	// If this is an immediate call, call now.
	return function() {
		clearTimeout(timeout);				
		callNow = immediate && !timeout;
		timeout = setTimeout(later, wait);					
		if ( callNow ) func.apply( this, arguments );
	}

}
```

And this is how you use it.

```
var myFunc = debounce( (e) => {
	...do your thang.
});

window.addEventListener('keyup', myFunc);
```

### Explanation

1. `const debounce =` - This is our debounce function, you could have called it whatever you want. What’s important, is what it’s doing on the inside.
2. `let timeout` - This represents the timer that we will be creating using _setTimeout_.
3. `const later` - This is the method that would get called in the event that a timer ran out of time. We’ve added a line to account for any calls that have an immediate flag. That’s handled outside of this function, and we don’t want it called more than once.
4. `return function()...` - This is the actual debounce portion of our code. Here we are doing a few things:
	1. `clearTimeout( timeout )` -  _clearTimeout_ takes a timerId parameter, and clears that timer, in this case  our _timeout_ variable.
	note: _setTimeout_ returns an id of the timer it represents.
	2. `callNow = immediate && !timeout` - This is boolean variable for determining whether or not to make the call immediately.
	3. `timeout = setTimeout(later,wait)` - Make the next timer and set it to our variable _timeout_. This essentially replaces the previously discarded timer with a new one.


### Outro
Debouncing is one of those things that seem very complex at first, but is actually great thing to add that can greatly improve the performance of your applications. Think of it this way, you won’t have a million function calls  every time that event is fired off.  

I’m hoping this helped you, if it did, or didn’t please leave a comment below, and thank you so much for your time & for visiting _code haven with Gabe_.
