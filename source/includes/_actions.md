# Actions

## maera/admin/save

```php
<?php

function my_custom_action() {
    // do something when the customizer is saved.
}
add_action( 'maera/admin/save', 'my_custom_action' );
?>
```

This action is run when the admin page is saved.
An example usage is when we import the customizer options in a shell that uses a compiler. In that case, when we perform the import we may want to trigger the shell's compiler so that the stylesheets get re-generated, caches dumped etc.


## maera/shell/include_modules

```php
<?php

function my_custom_action() {
    // Include the my-file.php file when the shell starts.
    include_once( dirname( __FILE__ ) . '/includes/my-file.php' );
}
add_action( 'maera/shell/include_modules', 'my_custom_action' );
?>
```

This action is run when the shell is being initialized.

## Content-injecting actions

```php
<?php
/**
* This function simply echoes some content.
* In this example we inject it at the top of single posts
* using the maera/single/top action.
* You can similarty inject your content wherever you need
* using any of the other actions available to you.
*/
function my_custom_action() { ?>
    <div class="alert">
        <p>This is a single post</p>
    </div>
    <?php
}
add_action( 'maera/single/top', 'my_custom_action' );
?>
```

* `maera/wrap/before`
* `maera/wrap/after`
* `maera/content/before`
* `maera/content/after`
* `maera/main/before`
* `maera/main/after`
* `maera/page/pre_content`
* `maera/page/after_content`
* `maera/entry/footer`
* `maera/teaser/start`
* `maera/single/top`
* `maera/single/pre_content`
* `maera/single/after_content`
* `maera/in_article/top`
* `maera/footer/before`
* `maera/footer/start`
* `maera/footer/content`
* `maera/footer/end`
* `maera/footer/after`
* `maera/header/before`
* `maera/header/after`
* `maera/index/begin`
* `maera/index/end`
* `maera/in_loop/begin`
* `maera/in_loop/end`
