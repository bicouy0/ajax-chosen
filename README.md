# Ajax-Chosen

This project is an addition to the excellent [Chosen jQuery plugin](https://github.com/harvesthq/chosen) that makes HTML input forms more friendly.  Chosen adds search boxes to `select` HTML elements. This extends those search boxes to work with ajax requests. 

This script bootstraps the existing Chosen plugin without making any modifications to the original code. 

## How to Use

This plugin exposes a new jQuery function named `ajaxChosen` that we call on a `select` element. 

The first argument is an option parameter for the plugin specific parameters

ajaxChosen specific parameters are: 

minLength: this is the number of characters that need to be typed before search occurs. Default is 3.
queryParameter: this is the query parameter that is used for your search. Default is 'term'. 
queryLimit: this is the max number of items that your server will ever return, and it is used for client side caching. Default is 10. 
data: this is what jQuery uses to convey your query. Feel free to add additional params, such as limits and sort orders. Default is {}


The second argument is a callback that the plugin uses to get option elements. The callback is expected to make ajax call, it receives as first argument the options of plugin and must call its second argument to pass the values back to the plugin, e.g. if it were a list of states it would be
	
	states[state.id] = state.name;

which would become

	<option value="state.id">state.name</option>


## Example Code

``` js
$("#example-input").ajaxChosen({
    minLength: 2,
    delay: 300
}, function (options, response) {
    $.getJSON("/ajax-chosen/data.php", {q: options.term}, function (data) {

	    var terms = {};
			
	    $.each(data, function (i, val) {
		    terms[i] = val;
	    });
			
	    response(terms);
    });
});
```

Note that this will hit the url: /ajax-chosen/data.php?q=WHATEVS (assuming you're searching for whatevs...)
