# Shells

## What Shells are

Shells can be built as plugins and allow you to completely custom-code your own theme, overriding the default templates of the theme as well as its stylesheets and other scripts.

The default templates of the theme already include a lot of hooks and actions you can use, allowing for a sane markup, SEO-optimization and easy to modify using a [shell macros](/docs/shells/macros/) file.

## Building a Shell

Maera makes it easy to create new shells.

The recommended method to create a new shell is by writing a custom WordPress plugin.

The first step will be to create our main plugin file. In this case we will call this file `shell.php` and add the following code in it:

```php
<?php
/*
Plugin Name:         Example Shell
Description:         This is just an example of how to build a shell plugin for the Maera theme
Version:             0.1
Author:              me
Author URI:          http://example.com
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
        // This will be used in the shell selection option in the dashboard
        'label' => 'Example',

        // The name of our Shell Class.
        // We're going to include everything concerning our shell there.
        'class' => 'Maera_Example_Shell',

    );

    return $shells;

}
// Add our shell to the list of available shells
add_filter( 'maera/shells/available', 'maera_shell_bootstrap_include' );

/**
 * Next we will build our main shell class which will contain all the functionality of our shell.
 * We will be loading our stylesheets, scripts and anything else required by our shell here.
 */

/**
 * The Shell
 */
class Maera_Example_Shell {

    private static $instance;

    /**
     * Add any actions & filters we may be using.
     * This is where you'll probably be executing any custom functions that you add to this class.
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

You can extend that class and create your own methods and functions but the above code is the bare minimum to create and use a new shell.
From that point on it's all up to your inspiration!

## Shell Macros

There are many CSS frameworks out there like for example Bootstrap, Foundation, Pure and lots more. Each one of them defines a grid, buttons, alert messages and a lot of other things using its unique HTML structure, CSS class names etc.

We wanted to have a mechanism that will allow our plugins to be framework-agnostic, so if you write something in a plugin it can work no matter what shell/css-framework you use on your site.

To that end we are using [twig macros](http://twig.sensiolabs.org/doc/tags/macro.html).  
Macros are a way to standardize some common functionalities that most frameworks have (such as for example a grid or buttons) while at the same time allowing us to easily change these when creating a new shell.  
You can see a list of the default macros in [the github repository](https://github.com/presscodes/maera/tree/master/views/macros).  
For example the macro to create a new row in the core shell looks like this:

```twig
{% macro row_open(element, id, extra_classes, properties) %}
    <{{ element|default('div') }} class="g{{ extra_classes ? ' '~extra_classes : '' }}"{{ id ? ' id="'~id~'"' : '' }}{{ properties ? ' '~properties : '' }}>
{% endmacro %}
```

If you want to use this macro in a twig custom twig file in your templates you will first have to import it by adding this line at the top of your twig file:

```twig
{% from 'row_open.html.twig' import row_open as row_open %}
```
and then you can use it to open a row like this:

```twig
{{ row_open( 'div', 'content', 'bg' ) }}
```
The above will have this as a result:

```html
<div class="g bg" id="content">
```

In addition, we also have shortcode-like syntax that can be entered wherever you want. The available functions that you can use this way are:

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

Example scenario: A row that has a 3-column layout inside. The 1st column is half the width of the page, the 2nd column is a 3rd of the page width and the 3rd column is 1 6th of the page width. The 1st and 2nd columns also have a button in them:  

```html
<div class="[maera_grid_row_class]">
    <div class="[maera_grid_col_6]">
        <p>This column has a width of 6/12 (half the page/content)</p>
        <a class="[maera_button_success_small]" href="#">small green button</a>
    </div>
    <div class="[maera_grid_col_4]">
        <p>This column is 4/12 of the page/content (1 3rd of the width of its parent element)</p>
        <a class="[maera_button_danger_large]" href="#">large red button</a>
    </div>
    <div class="[maera_grid_col_2]">
        <p>This column is 2/12 of the page/content width. That's too narrow and rarely used</p>
    </div>
</div>
```

Though the above example is pretty simplistic, it gives you an idea of how simple it is to implement these in your own templates. The great thing about them is that you can also use them **in your content**, so you can simply use them when using the WordPress content editor. This will allow you to have your content play nicely no matter which shell you're using. So you can be using [Bootstrap](http://getbootstrap.com/) now and [foundation](http://foundation.zurb.com/) next month, or even [purecss](http://purecss.io/) and your content and customisations will look nice wherever you are.
