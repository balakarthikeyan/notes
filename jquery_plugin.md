Create a wrapper function which allows me to use $ and not cause conflicts with other JavaScript frameworks that may be included. It is simple and looks like this:

```
(function ($) {
	// safe to use $ here and not cause conflicts
})(jQuery);
```

## Create and Name Your Plugin Function

Next, I created the function inside which my plugin will reside. Whatever you name this function will be how you and others use your plugin.

```
(function ($) {
	$.fn.toggler = function () {
		// the plugin functionality goes in here
	}
})(jQuery);
```

I have emboldened the function name in the code above to make it obvious. This means that to use the plugin I am creating, someone would have to do `$('#some_selector').toggler()`.

## Allow chainability

The next thing to keep in mind is that because jQuery takes any selector, you could be dealing with one or more objects. This means you need to be sure that your plugin loops through each element matched by the selector and applies the plugin functionality.

```
(function ($) {
	$.fn.toggler = function () {
		return this.each(function () {
			// apply plugin functionality to each element
		});
	}
})(jQuery);
```

The other thing you will notice about the addition above is that I use return. One of the cool things about jQuery is the chainability (ie: `$('#some_div').hide().remove()`). Returning the this.each iteration allows your plugin to be chained with other jQuery operations and plugins.

## Start Building Plugin Functionality

The goal of the plugin I was creating was to allow saying that a particular link is a toggler. So what is a toggler you ask? A toggler is a link that toggles the visibility of another element on the page. The connection of the link to the element is by using the href. By setting the link's href to anchor to the id of another element, we get two things for free. First, if JavaScript is not enabled, the link will still anchor to the element on the page. Second, we know which element the link is suppose to toggle display for. Take the following html for example:

The default behavior of the toggler link is to anchor to the Togglee div wherever it is on the page. We will then use the jQuery plugin we are making to hide the togglee div on page load and then toggle its display each time the toggler link is clicked. First, let's hide the togglee div and add the click observer to the toggler link.

```
(function ($) {
	$.fn.toggler = function () {
		return this.each(function () {
			this.togglee = $($(this).attr('href')).hide();
			$(this).click(onClick);
		});
	}
})(jQuery);
```

The first bold line sets a togglee attribute on the toggler that is equal to the togglee jQuery element. Note the` $(this).attr('href')`. Using the html from above, this would return #foo. `$('#foo')` is equivalent to a jQuery object of the togglee div. We call `hide()` to hide the div by default. Because jQuery allows chaining, the hide() call returns the togglee jQuery object and we save a line of code. The long hand way of writing that line would be something like this:
```
this.togglee = $($(this).attr('href'));
this.togglee.hide();
```

Because jQuery allows chaining, lines like the ones above can be combined and in my opinion remain readable. Chaining too many things together can lower readability and the obviousness of intent though, so always practice safe chaining.

## Observing Clicks

The `$(this).click(onClick);` line above tells the toggler (which is `$(this)`) to run the function onClick whenever it gets clicked. We haven't yet created the onClick function so let's do that now.

```
(function ($) {
	$.fn.toggler = function () {
		return this.each(function () {
			this.togglee = $($(this).attr('href')).hide();
			$(this).click(onClick);
		});

		function onClick() {
			this.togglee.toggle()
			return false;
		}
	}
})(jQuery);
```

this is automatically set to the element that fired the onClick, by jQuery, which is our toggler link. Because we assigned the togglee attribute to the toggler link, we can simple call this.togglee.toggle() and it will toggle the display of the togglee div. The return false tells the togglee link to not actually fire the normal event, which would append #foo to the window location.

## Allowing For Options

At this point, we could be done but why not allow some options for our plugin. In this case, we'll add the option to make the togglee div animate with a slide instead of simply showing and hiding. Check out the bold lines in the cold sample below to see what is needed to add options.
```
(function ($) {
	$.fn.toggler = function (options) {

		var opts = $.extend({}, $.fn.toggler.defaults, options);

		return this.each(function () {
			this.togglee = $($(this).attr('href')).hide();
			$(this).click(onClick);
		});

		function onClick() {
			this.togglee[opts.animate ? 'slideToggle' : 'toggle']();
			return false;
		}
	}

	$.fn.toggler.defaults = {
		animate: false
	}
})(jQuery);
```

The first bold line sets the opts variable and uses jQuery's extend function to do so. Basically, `$.extend` is a merge tool. In plain english here is what happens: it merges `$.fn.toggler.defaults` on top of {} and then merges options on top of the first merge. It guarantees that our opts variable will be a hash ({}), will have some defaults (`$.fn.toggler.defaults`) and yet can be overridden on a per case basis with options passed in by you, the developer (options).

In the second bold line, we tell togglee to call slideToggle if opts.animate is true, otherwise just call toggle. This uses the ternary operator and some cool JavaScript functionality. If opts.animate is true, it returns 'slideToggle', otherwise it returns 'toggle'. This means if opts.animate is true, you would end up with `this.togglee['slideToggle']()`, which is the sweet functionality I referenced above that JavaScript natively provides. `this.togglee['slideToggle']` is a reference to the slideToggle function and when you apply the parenthesis it will invoke the slideToggle function. I feel this way is more succinct, is still readable and again, it saves you a few lines and an if/else statement. Not a big deal, but worth it in this case.

The third and final emboldened line just sets the defaults that get used in the first line if no options are provided by you when you call toggler on some elements.

## Example Uses With/Without Options

Now that we have options, you can call the toggler function any of the following ways and it will work.
```
// will do simple toggle on click
$('a.toggler').toggler();

// will use slideToggle instead of simple toggle on click
$('a.toggler.animate').toggler({
	animate: true
});
```
That is pretty much it. You can now create a jQuery plugin that is jQuery-ish and has the ability to have options that can be adjusted on the fly, by you and the people who use your amazing plugin. The only other thing I would mention is most people seem to name their plugins jquery.{plugin name}.js. For example, I named this plugin jquery.toggler.js.