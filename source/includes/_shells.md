# Shells

## What Shells are

Shells can be built as plugins and allow you to completely custom-code your own theme, overriding the default templates of the theme as well as its stylesheets and other scripts.

The default templates of the theme already include a lot of hooks and actions you can use, allowing for a sane markup, SEO-optimization and easy to modify using a [shell macros](shell-macros) file.

## Building a Shell

```php
<?php

/*
Plugin Name:         Example Shell
Description:         This is just an example of how to build a shell plugin for the Maera theme
Version:             0.1
Author:              me
*/

define( 'MAERA_EXAMPLE_SHELL_URL', plugins_url( '', __FILE__ ) );
define( 'MAERA_EXAMPLE_SHELL_PATH', dirname( __FILE__ ) );

/**
 * Include the shell
 */
function maera_shell_example_include( $shells ) {

    // Add our shell to the array of available shells
    $shells[] = array(

        // Alphanumeric (lowercase name of our shell)
        'value' => 'example',

        // The full name of our shell.
        'label' => 'Example',

        // The name of our Shell Class.
        'class' => 'Maera_Example_Shell',

    );

    return $shells;

}
// Add our shell to the list of available shells
add_filter( 'maera/shells/available', 'maera_shell_bootstrap_include' );

/**
 * Next we will build our main shell class
 * which will contain all the functionality of our shell.
 * We will be loading our stylesheets, scripts
 * and anything else required by our shell here.
 */

/**
 * The Shell
 */
class Maera_Example_Shell {

    private static $instance;

    /**
     * Add any actions & filters we may be using.
     * These are executed as soon as the class is instantiated.
     */
    private function __construct() {

        do_action( 'maera/shell/include_modules' );

        // CAUTION: DO NOT DELETE THIS.
        if ( ! defined( 'MAERA_SHELL_PATH' ) ) {
            define( 'MAERA_SHELL_PATH', dirname( __FILE__ ) );
        }

    }

    /**
     * This is required to instantiate our shell.
     * CAUTION, DO NOT DELETE THIS FUNCTION.
     */
    public static function get_instance() {
        if ( null == self::$instance ) {
            self::$instance = new self;
        }

        return self::$instance;
    }

}
?>
```

Maera makes it easy to create new shells.

The recommended method to create a new shell is by writing a custom WordPress plugin.

The first step will be to create our main plugin file. In this case we will call this file `shell.php` and add the following code in it:

You can extend that class and create your own methods and functions but the above code is the bare minimum to create and use a new shell.
From that point on it's all up to your inspiration!

## Shell Macros

```html
{% from 'grid_classes.html.twig' import container as container %}
{% from 'grid_classes.html.twig' import row as row %}
{% from 'column_classes.html.twig' import column_classes as column_classes %}

<div class="{{ container }}">
    <div class="{{ row }}">

        <div class="{{ column_classes(
            [
                {'mobile':12},
                {'tablet':6},
                {'medium':8},
                {'large':9}
            ]
        ) }}">
            <p>12 columns on a mobile,</p>
            <p>6 columns on a tablet</p>
            <p>8 columns on a normal screen</p>
            <p>9 columns on large screens</p>

            <p>The actual implementation depends on the shell.</p>
        </div>
        <div class="{{ column_classes(
            [
                {'mobile':12},
                {'tablet':6},
                {'medium':4},
                {'large':3}
            ]
        ) }}">
            <p>12 columns on a mobile</p>
            <p>6 columns on a tablet</p>
            <p>4 columns on a normal screen</p>
            <p>3 columns on large screens</p>

            <p>The actual implementation depends on the shell.</p>
        </div>
    </div>
</div>
```

There are many CSS frameworks out there like for example Bootstrap, Foundation, Pure and lots more. Each one of them defines a grid, buttons, alert messages and a lot of other things using its unique HTML structure, CSS class names etc.

We wanted to have a mechanism that will allow our plugins to be framework-agnostic, so if you write something in a plugin it can work no matter what shell/css-framework you use on your site.

To that end we are using [twig macros](http://twig.sensiolabs.org/doc/tags/macro.html).  
Macros are a way to standardize some common functionalities that most frameworks have (such as for example a grid or buttons) while at the same time allowing us to easily change these when creating a new shell.  
You can see a list of the default macros in [the github repository](https://github.com/presscodes/maera/tree/master/views/macros).  

As you can see on the example on the right, you **first have to import the macros** and then use them.

On this code snippet we're first creating a container div. In that we're adding a row, and in that row 2 columns. The first column is wider and the second one is narrower. We have the flexibility to define separate width on a per-screen-size basis, or you can only define the medium size and the rest will be automatically filled-in.

If we were using [Bootstrap 3](http://getbootstrap.com/) for example on our shell, the result would look like this:

`<div class="container"><div class="row"><div class="col-xs-12 col-sm-6 col-md-8 col-lg-9"><p>12 columns on a mobile,</p><p>6 columns on a tablet</p><p>8 columns on a normal screen</p><p>9 columns on large screens</p><p>The actual implementation depends on the shell.</p></div><div class="col-xs-12 col-sm-6 col-md-4 col-lg-3"><p>12 columns on a mobile</p><p>6 columns on a tablet</p><p>4 columns on a normal screen</p><p>3 columns on large screens</p><p>The actual implementation depends on the shell.</p></div></div></div>`

If you're building a site for yourself or a client you may not need these, but as a good practice we recommend you do use them so that your site & templates will still work and be properly styled even if you completely change the shell & CSS framework you're using.
No matter what happens, your templates will always use the proper CSS classes provided by your shell.

## Shortcode Macros

```html
<div class="[maera_grid_container_class]">
    <div class="[maera_grid_row_class]">
        <div class="[maera_grid_col_8]">
            <p>8 columns</p>
            <p>The actual implementation depends on the shell.</p>
        </div>
        <div class="[maera_grid_col_4]">
            <p>4 columns</p>
            <p>The actual implementation depends on the shell.</p>
        </div>
    </div>
</div>
```
In addition to normal macros, we also have shortcode-like syntax that can be entered wherever you want.
In some cases they are not as versatile as their corresponding macros, but they are a lot easier to implement and in most cases they will achieve the exact same result.
The great thing about them is that you can also use them **in your content**, so you can simply use them when using the WordPress content editor. This will allow you to have your content play nicely no matter which shell you're using. So you can be using [Bootstrap](http://getbootstrap.com/) now and [foundation](http://foundation.zurb.com/) next month, or even [purecss](http://purecss.io/) and your content and customisations will look nice wherever you are.

If for some reason you need more flexibility then please take a look at the [Shell Macros](#shell-macros) section.

The available functions that you can use this way are:

`[maera_grid_container_open]` - opens a container div  
`[maera_grid_container_close]` - closes a container div  
`[maera_grid_container_class]` - the class of the container

`[maera_grid_row_open]` - open a row div  
`[maera_grid_row_close]` - closes a row div  
`[maera_grid_row_class]` - the class of the row

`[maera_grid_col_1]` - the column classes for width 1/12  
`[maera_grid_col_2]` - the column classes for width 2/12  
`[maera_grid_col_3]` - the column classes for width 3/12  
`[maera_grid_col_4]` - the column classes for width 4/12  
`[maera_grid_col_5]` - the column classes for width 5/12  
`[maera_grid_col_6]` - the column classes for width 6/12  
`[maera_grid_col_7]` - the column classes for width 7/12  
`[maera_grid_col_8]` - the column classes for width 8/12  
`[maera_grid_col_9]` - the column classes for width 9/12  
`[maera_grid_col_10]` - the column classes for width 10/12  
`[maera_grid_col_11]` - the column classes for width 11/12  
`[maera_grid_col_12]` - the column classes for width 12/12  

`[maera_button_default_extra_small]` - classes for default, extra-small buttons  
`[maera_button_default_small]` - classes for default, small buttons  
`[maera_button_default_medium]` - classes for default, medium buttons  
`[maera_button_default_large]` - classes for default, large buttons  
`[maera_button_default_extra_large]` - classes for default, extra-large buttons  

`[maera_button_primary_extra_small]` - classes for primary, extra-small buttons  
`[maera_button_primary_small]` - classes for primary, small buttons  
`[maera_button_primary_medium]` - classes for primary, medium buttons  
`[maera_button_primary_large]` - classes for primary, large buttons  
`[maera_button_primary_extra_large]` - classes for primary, extra-large buttons  

`[maera_button_success_extra_small]` - classes for success, extra-small buttons  
`[maera_button_success_small]` - classes for success, small buttons  
`[maera_button_success_medium]` - classes for success, medium buttons  
`[maera_button_success_large]` - classes for success, large buttons  
`[maera_button_success_extra_large]` - classes for success, extra-large buttons  

`[maera_button_info_extra_small]` - classes for info, extra-small buttons  
`[maera_button_info_small]` - classes for info, small buttons  
`[maera_button_info_medium]` - classes for info, medium buttons  
`[maera_button_info_large]` - classes for info, large buttons  
`[maera_button_info_extra_large]` - classes for info, extra-large buttons  

`[maera_button_warning_extra_small]` - classes for warning, extra small buttons  
`[maera_button_warning_small]` - classes for warning, small buttons  
`[maera_button_warning_medium]` - classes for warning, medium buttons  
`[maera_button_warning_large]` - classes for warning, large buttons  
`[maera_button_warning_extra_large]` - classes for warning, extra-large buttons  

`[maera_button_danger_extra_small]` - classes for danger, extra-small buttons  
`[maera_button_danger_small]` - classes for danger, small buttons  
`[maera_button_danger_medium]` - classes for danger, medium buttons  
`[maera_button_danger_large]` - classes for danger, large buttons  
`[maera_button_danger_extra_large]` - classes for danger, extra-large buttons  

`[maera_button_link_extra_small]` - classes for link, extra-small buttons  
`[maera_button_link_small]` - classes for link, small buttons  
`[maera_button_link_medium]` - classes for link, medium buttons  
`[maera_button_link_large]` - classes for link, large buttons  
`[maera_button_link_extra_large]` - classes for link, extra-large buttons  
