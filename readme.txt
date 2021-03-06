=== Safe Function Call ===
Contributors: coffee2code
Donate link: https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=6ARCFJ9TX3522
Tags: function, template, plugin, error, coffee2code
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html
Requires at least: 1.5
Tested up to: 4.3
Stable tag: 1.2.3

Safely and easily call functions that may not be available (such as those provided by a plugin that gets deactivated)


== Description ==

Safely call a function, class method, or object method in a manner that doesn't generate errors if those plugins cease to exist.

Various helper functions are provided that provide handy variations of this theme:

* `_sfc()`: Safely call a function and get its return value
* `_sfce()`: Safely call a function and echo its return value
* `_sfcf()`: Safely call a function; if it doesn't exist, then a fallback function (if specified) is called
* `_sfcm()`: Safely call a function; if it doesn't exist, then echo a message (if provided)

Let's assume you had something like this in a template:

`<?php list_cities( 'Texas', 3 ); ?>`

If you deactivated the plugin that provided `list_cities()`, your site would generate an error when that template is accessed.

You can instead use `_sfc()`, which is provided by this plugin to call other functions, like so:

`<?php _sfc( 'list_cities', 'Texas', 3 ); ?>`

That will simply do nothing if the `list_cities()` function is not available.

If you'd rather display a message when the function does not exist, use `_sfcm()` instead, like so:

`<?php _sfcm( 'list_cities', 'The cities listing is temporarily disabled.', 'Texas', 3 ); ?>`

In this case, if `list_cities()` is not available, the text "The cities listing is temporarily disabled." will be displayed.

If you'd rather call another function when the function does not exist, use _sfcf() instead, like so:

`<?php
	function unavailable_function_handler( $function_name ) { echo "The function $function_name is not available."; }
	_sfcf( 'nonexistent_function', 'unavailable_function_handler' );
?>`

In the event you want to safely call a function and echo its value, you can use `_sfce()` like so:

`<?php _sfce( 'largest_city', 'Tx' ); ?>`

Which is roughly equivalent to doing :

`<?php if function_exists( 'largest_city' ) { echo largest_city( 'Tx' ); } ?>`

Links: [Plugin Homepage](http://coffee2code.com/wp-plugins/safe-function-call/) | [Plugin Directory Page](https://wordpress.org/plugins/safe-function-call/) | [Author Homepage](http://coffee2code.com)


== Installation ==

1. Unzip `safe-function-call.zip` inside the `/wp-content/plugins/` directory (or install via the built-in WordPress plugin installer)
1. Activate the plugin through the 'Plugins' admin menu in WordPress
1. Use any of the four functions provided by this plugin as desired


== Frequently Asked Questions ==

= Do the functions provided by this plugin capture any error messages generated by the specified function? =

No.

= Why would I use any of these functions instead of using `function_exists()`/`method_exists()` directly? =

The functions provided by this plugin provide a more concise syntax for checking for function existence (but it does use `function_exists()`/`method_exists()` under the hood). `_sfce()` will both echo and return the echoed value, which may be of use in certain circumstances.  And also, since the callback to be safely called is passed as an argument, it can be easily and more concisely parameterized.

= Does this plugin include unit tests? =

Yes.


== Template Tags ==

The plugin provides four functions for your use. *Note: These functions are not limited to use in templates*

= Functions =

* `<?php function _sfc($callback) ?>`
This will safely invoke the specified callback. You can specify an arbitrary number of additional arguments that will get passed to it. If the callback does not exist, nothing is displayed and no error is generated.

* `<?php function _sfce($callback) ?>`
The same as `_sfc()` except that it echoes the return value of the callback before returning that value.

* `<?php function _sfcf($callback, $fallback_callback = '') ?>`
The same as `_sfc()` except that it invokes the fallback callback (if it exists) if the callback does not exist.  `$function_name_if_missing()` is sent `$function_name` as its first argument, and then subsequently all arguments that would have otherwise been sent to `$function_name()`.

* `<?php function _sfcm($callback, $message_if_missing = '') ?>`
The same as `_sfc()` except that it displays a message (the value of `$message_if_missing`), if the callback does not exist.

= Arguments =

* `$callback`
A string representing the name of the function to be called, or an array of a class or object and its method (as can be done for `add_action()`/`add_filter()`)

* `$message_if_missing`
(For `_sfcm()` only.)  The message to be displayed if `$function_name()` does not exist as a function.

* `$fallback_callback`
(For `_sfcf()` only.)  The function to be called if the callback does not exist.

= Examples = 

* `<?php _sfc('list_cities', 'Texas', 3); /* Assuming list_cities() is a valid function */ ?>`
"Austin, Dallas, Fort Worth"

* `<?php _sfc(array('Cities', 'list_cities'), 'Texas', 3); /* Assuming list_cities() is a valid function in the 'Cities' class */ ?>`
"Austin, Dallas, Fort Worth"

* `<?php _sfc(array($obj, 'list_cities'), 'Texas', 3); /* Assuming list_cities() is a valid function in the object $obj */ ?>`
"Austin, Dallas, Fort Worth"

* `<?php _sfc('list_cities', 'Texas', 3); /* Assuming list_cities() is not a valid function */ ?>`
""

* `<?php _sfcm('list_cities', 'Texas', 'Unable to list cities at the moment', 3); /* Assuming list_cities() is a valid function */ ?>`
"Austin, Dallas, Fort Worth"

* `<?php _sfcm('list_cities', 'Texas', 'Unable to list cities at the moment', 3); /* Assuming list_cities() is not a valid function */ ?>`
"Unable to list cities at the moment"

* `<?php _sfce('largest_city', 'Tx'); /* Assuming largest_city() is a valid function that does not echo/display its return value */ ?>`
"Houston"

* `<?php
	function unavailable_function_handler( $callback ) {
		echo "Sorry, but the function {$callback}() does not exist.";
	}
	_sfcf('nonexistent_function', 'unavailable_function_handler');
	?>`


== Changelog ==

= 1.2.3 (2015-08-04) =
* Note compatibility through WP 4.3+

= 1.2.2 (2015-02-11) =
* Note compatibility through WP 4.1+
* Update copyright date (2015)

= 1.2.1 (2014-08-25) =
* Die early if script is directly invoked
* Minor plugin header reformatting
* Minor code reformatting (spacing)
* Change documentation links to wp.org to be https
* Note compatibility through WP 4.0+
* Add plugin icon

= 1.2 (2013-12-19) =
* Add support for full callback usage
* Add `__sfc_is_valid_callback()` to validate callbacks; use it in all functions
* Add unit tests
* Substantial changes to inline documentation
* Substantial changes to documentation
* Minor code formatting tweak (add curly braces)
* Note compatibility through WP 3.8+
* Update copyright date (2014)
* Change donate link
* Add banner

= 1.1.7 =
* Note compatibility through WP 3.5+
* Update copyright date (2013)

= 1.1.6 =
* Re-license as GPLv2 or later (from X11)
* Add 'License' and 'License URI' header tags to readme.txt and plugin file
* Remove ending PHP close tag
* Miscellaneous readme.txt changes
* Update copyright date (2012)
* Note compatibility through WP 3.4+

= 1.1.5 =
* Note compatibility through WP 3.3+
* Minor code documentation reformatting in readme.txt (spacing)

= 1.1.4 =
* Note compatibility through WP 3.2+
* Minor documentation reformatting in readme.txt
* Fix plugin homepage and author links in description in readme.txt

= 1.1.3 =
* Add link to plugin homepage to readme.txt

= 1.1.2 =
* Note compatibility through WP 3.1+
* Update copyright date (2011)

= 1.1.1 =
* Wrapped functions in if(function_exists()) checks
* Note compatibility with WP 3.0+
* Change description
* Minor code reformatting (spacing)
* Remove documentation and instructions from top of plugin file (all of that and more are contained in readme.txt)
* Add Upgrade Notice section to readme.txt

= 1.1 =
* Add new template function _sfcf() to allow calling a function when the intended function isn't available
* Add PHPDoc documentation
* Minor formatting tweaks
* Note compatibility with WP 2.9+
* Update copyright date
* Update readme.txt (including adding Changelog)

= 1.0 =
* Initial release


== Upgrade Notice ==

= 1.2.3 =
Trivial update: noted compatibility through WP 4.3+

= 1.2.2 =
Trivial update: noted compatibility through WP 4.1+ and updated copyright date

= 1.2.1 =
Trivial update: noted compatibility through WP 4.0+; added plugin icon.

= 1.2 =
Recommended update: added support for full callback usage; improved documentation; added unit tests; noted compatibility through WP 3.8+

= 1.1.7 =
Trivial update: noted compatibility through WP 3.5+

= 1.1.6 =
Trivial update: noted compatibility through WP 3.4+; explicitly stated license

= 1.1.5 =
Trivial update: noted compatibility through WP 3.3+  and minor code documentation formatting changes (spacing)

= 1.1.4 =
Trivial update: noted compatibility through WP 3.2+ and minor code formatting changes (spacing)

= 1.1.3 =
Trivial update: documentation tweaks

= 1.1.2 =
Trivial update: noted compatibility through WP 3.1+ and updated copyright date

= 1.1.1 =
Minor update. Wrapped functions in if(function_exists()) checked; noted compatibility with WP 3.0+.
