# Filters

## maera/admin/options

This filter will add items to the Admin Theme Options page.

You may want to use this filter to add aditional options. However, theme specific customization options should use the customizer and not the options page.

## maera/container_class

```php
<?php
function my_container_class() {

    $class_setting = get_theme_mod( 'my_theme_mod',1 );

    if ( $class_setting  == 1 ) {
        // Bootstrap responsive fixed container class.
        $main_class = 'container';
    } else {
        // Bootstrap fluid full-width container class.
        $main_class = 'container-fluid';
    }

    return $main_class;

}
add_filter( 'maera/container_class', 'maera_container_class' );
?>
```
This filter will change or add the class to the main container.
You may want to use this filter to change the width of the container or to append a certain CSS class.

## maera/content_width

```php
<?php
function my_content_width() {

    if ( ! isset( $content_width ) ) {
        $content_width = 960;
    }

    return $content_width;

}
add_filter( 'maera/content_width', 'maera_' );
?>
```

You may want to use this filter to change the default width of the container.

The content_width can be used in your `twig` files using `{{ content_width }}` and in some shells we use it to calculate for example the width of featured images.

## maera/image/height

```php
<?php
// Set the height of featured images to 200px
function my_featured_images_height() {
    return 200;
}
add_action( 'maera/image/height', 'my_featured_images_height' );
?>
```

You may want to use this filter to change the height of featured images. This may be useful when creating fluid, concise layouts.

## maera/image/width

```php
<?php

// Set the width of featured images to 450px
function my_featured_images_width() {
    return 450;
}
add_action( 'maera/image/height', 'my_featured_images_width' );

?>
```
You may want to use this filter to change the width of featured images. This may be useful when creating fluid, concise layouts.

## maera/section_class/primary

```php
<?php

function my_sidebar_primary_classes( $class ) {
    return $class . ' primary col-sm-6 col-md-4 col-lg-3';
}
add_action( 'maera/section_class/primary', 'my_sidebar_primary_classes' );

?>
```
Allows you to add classes to the primary sidebar.

## maera/section_class/secondary

Allows you to add classes to the secondary sidebar. Usage is exactly the same as the [maera/section_class/primary](#maerasection_classprimary) action.

## maera/section_class/wrapper


Allows you to add classes to a wrapper div that includes the content area and the primary sidebar. Particularty useful if you're trying to achieve a template that has 3 columns and you need to float the content and sidebar to the right. Usage is exactly the same as the [maera/section_class/primary](#maerasection_classprimary) action.

## maera/shells/available

```php
<?php

/**
 * Include the shell
 */
function maera_shell_core_include( $shells ) {

    // Add our shell to the array of available shells
    $shells[] = array(
        'value' => 'core',
        'label' => 'Core',
        'class' => 'Maera_Shell_Core',
    );

    return $shells;

}
add_filter( 'maera/shells/available', 'maera_shell_core_include' );

?>
```
You can use this filter to add your own shell in the list of available shells so that users can select and activate it.

More info can be found on the [Building a Shell](#building-a-shell) page.

## maera/styles

```php
<?php
function custom_header_css( $styles ) {

    $image_url = get_header_image();
    if ( is_singular() && has_post_thumbnail() ) {
        $image_array = wp_get_attachment_image_src( get_post_thumbnail_id(), 'full' );
        $image_url = $image_array[0];
    }

    if ( empty( $image_url ) ) {
        return $styles;
    } else {
        return $styles . '.page-header:before{ background: url("' . $url . '") no-repeat center center; }';
    }

}
add_action( 'maera/styles', 'custom_header_css' );

?>
```
If you need to add some custom CSS to your page, you can use this filter.

## maera/stylesheet/url

```php
<?php

function my_stylesheet_url() {
    return get_stylesheet_directory_uri() . '/assets/style.css';
}
add_filter( 'maera/stylesheet/url', array( $this, 'my_stylesheet_url' ) );
?>
```
Changes the URL of the stylesheet to be loaded. You can use that if you're building your own shell to replace the default, empty stylesheet with your own.

## maera/stylesheet/ver

```php
<?php

/**
 * Get the file modification time of our stylesheet
 * and use it as the file version.
 */
function my_stylesheet_version() {
    return filemtime( get_stylesheet_directory() . '/assets/style.css' );
}
add_filter( 'maera/stylesheet/ver', array( $this, 'my_stylesheet_version' ) );
?>
```
Changes the version of the stylesheet for cache custing.

## Other filters (to be documented):
* maera/sidebar/footer
* maera/sidebar/primary
* maera/sidebar/secondary
* maera/timber/locations
* maera/title
* maera/teaser/mode
* maera/widgets/class
* maera/widgets/title/after
* maera/widgets/title/before
* maera/plugins/required
* maera/twig/placeholders
* maera/templates
* maera/template/plugin_compatibility
* maera/timber/context
* maera/timber/locations/roots
* maera/timber/locations
* maera/sidebar_template
* maera/admin/tabs
* maera/section_class/content
