# Templating

Maera is a WordPress theme and as such it follows the default WordPress [template hierarchy](http://wphierarchy.com/). However, we do have some extra stuff going on under the hood that we have found make life a lot easier both for skilled developers and for novice users that may not be php developers.

In addition to the default template files that you can use if you want, we also provide a "views" folder that contains "twig" files. You can learn more about the twig language by clicking on [this link](http://twig.sensiolabs.org/).
`twig` files have a structure and syntax a lot easier and more user-friendly than php. For example if you wanted to echo something in PHP, you would do this:

`<?php echo $foo; ?>`

In twig however it's a lot simpler than that:

`{{ foo }}`

You can learn more about the syntax and naming conventions by reading the [Timber Docs](https://github.com/jarednova/timber/wiki).

To add your own templates you can create a [Child Theme](href="http://codex.wordpress.org/Child_Themes) and either create custom php template files like you would do on every other WordPress theme, or custom twig templates.
The template hierarchy of the twig files is the same as the [default WordPress templates structure](href="http://wphierarchy.com/), simply by replacing the `.php` suffix with `.twig`.
You can keep your `.twig` files in the root of your child theme, or a `/views` folder if you want to keep things a little more organized.

## Rendering a twig file

> Rendering a twig file from a PHP template

```php
<?php Maera()->template->render( 'filename.twig' ); ?>
```

> adding custom context:

```php
<?php

// Get the global context
$context = Maera()->template->context();
// Add our custom context
// We'll be able to call it in our twigs
// using {{ my_text }}
$context['my_text'] = __( 'This is some custom context.' );
// Render the filename.twig file and include our context
Maera()->template->render( 'filename.twig', $context );

?>

```

If you want to render a `.twig` file and call it from a PHP template, you can use the `render` method.

Using `Maera()->template->render()` you can render your twig files and pass your own context to them. If no filename is defined then the theme will automatically choose the appropriate file from its [template hierarchy](#template-hierarchy).

When you use this method, any caching you've set on your admin settings will be automatically applied.

## Calling the theme header

> Call the header.php file

```php
<?php Maera()->template->header(); ?>
```

In normal WordPress themes you have to call the [`get_header()`](http://codex.wordpress.org/Function_Reference/get_header) function to call your theme's header. This loads the header.php file and its content.
In Maera, you can do the same using `Maera()->template->header();`.

Though you could simply use `get_header()`, using the above method will ensure your custom templates are ready for the future.

Right now our method simply calls `get_header()`, but we don't know what the future holds for us so if there are any improvements or customizations in the future, this will ensure you'll take advantage of them.

## Calling the theme footer

> Call the footer.php file

```php
<?php Maera()->template->footer(); ?>
```

In normal WordPress themes you have to call the [`get_footer()`](http://codex.wordpress.org/Function_Reference/get_footer) function to call your theme's footer. This loads the footer.php file and its content.
In Maera, you can do the same using `Maera()->template->footer();`.

Though you could simply use `get_footer()`, using the above method will ensure your custom templates are ready for the future.

Right now our method simply calls `get_header()`, but we don't know what the future holds for us so if there are any improvements or customizations in the future, this will ensure you'll take advantage of them.

## Template hierarchy

You can create additional `.twig` files in a child them (or a shell) if you need to override the default template for a post, a page, a taxonomy or whatever you want.

On the table below you can see the template hierarchy used by Maera for `.twig` template files.

<table>
    <th>
        <td>Content Template</td>
    </th>
    <tr>
        <th>Author Archives</th>
        <td>
            <p>author-{nicename}.twig</p>
            <p>author-{ID}.twig</p>
            <p>author.twig</p>
            <p>archive.twig</p>
            <p>index.twig</p>
        </td>
    </tr>
    <tr>
        <th>Category Archives</th>
        <td>
            <p>category-{slug}.twig</p>
            <p>category-{ID}.twig</p>
            <p>category.twig</p>
            <p>archive.twig</p>
            <p>index.twig</p>
        </td>
    </tr>
    <tr>
        <th>Custom Post Type Archives</th>
        <td>
            <p>archive-{post_type}.twig</p>
            <p>archive.twig</p>
            <p>index.twig</p>
        </td>
    </tr>
    <tr>
        <th>Custom Taxonomy Archives</th>
        <td>
            <p>taxonomy-{term}.twig</p>
            <p>taxonomy-{taxonomy}.twig</p>
            <p>archive.twig</p>
            <p>index.twig</p>
        </td>
    </tr>
    <tr>
        <th>Date Archives</th>
        <td>
            <p>date.twig</p>
            <p>archive.twig</p>
            <p>index.twig</p>
        </td>
    </tr>
    <tr>
        <th>Tag Archives</th>
            <td>
                <p>tag-{slug}.twig</p>
                <p>tag-{ID}.twig</p>
                <p>tag.twig</p>
                <p>archive.twig</p>
                <p>index.twig</p>
            </td>
    </tr>
    <tr>
        <th>Single Post</th>
        <td>
            <p>single-post.twig</p>
            <p>single.twig</p>
            <p>index.twig</p>
        </td>
    </tr>
    <tr>
        <th>Single Page</th>
        <td>
            <p>page-{slug}.twig</p>
            <p>page-{ID}.twig</p>
            <p>page.twig</p>
            <p>index.twig</p>
        </td>
    </tr>
</table>

## Sidebar Template Hierarchy

If you want to override the sidebar for a category or a page, you can do so easily by using a custom `.twig` template file for your sidebar.

You can use the table below to see what file-name you will have to use to achieve the results you're after.

<table>
    <th><td>Sidebar Template</td></th>
    <tr>
        <th>Author Archives</th>
        <td>
            <p>sidebar.twig</p>
        </td>
    </tr>
    <tr>
        <th>Category Archives</th>
    <td>
        <p>sidebar-category-{term_id}.twig</p>
        <p>sidebar-category.twig</p>
        <p>sidebar.twig</p>
    </td>
    </tr>
    <tr>
        <th>Custom Post Type Archives</th>
        <td>
        <p>sidebar-archive-{post_type}.twig</p>
        <p>sidebar.twig</p>
        </td>
    </tr>
    <tr>
        <th>Custom Taxonomy Archives</th>
        <td>
            <p>sidebar-term-{term_id}.twig</p>
            <p>sidebar-taxonomy-{taxonomy}.twig</p>
            <p>sidebar-taxonomy.twig</p>
            <p>sidebar.twig</p>
        </td>
    </tr>
    <tr>
        <th>Date Archives</th>
        <td>
            <p>sidebar-date.twig</p>
            <p>sidebar.twig</p>
        </td>
    </tr>
    <tr>
        <th>Tag Archives</th>
        <td>
            <p>sidebar-tag-{term_id}.twig</p>
            <p>sidebar-tag.twig</p>
            <p>sidebar.twig</p>
        </td>
    </tr>
    <tr>
        <th>Single Post</th>
        <td>
            <p>sidebar-{post-ID}.twig</p>
            <p>sidebar-post.twig</p>
            <p>sidebar-single.twig</p>
            <p>sidebar.twig</p>
        </td>
    </tr>
    <tr>
        <th>Single Page</th>
        <td>
            <p>sidebar-{post-ID}.twig</p>
            <p>sidebar-page.twig</p>
            <p>sidebar-single.twig</p>
            <p>sidebar.twig</p>
        </td>
    </tr>
</table>

## The stucture of a rendered page

We divided the pages in sub-files in ordet to make them more modular.
You can see the twig file template parts on the below diagram:
![Twig Template Parts](https://press.codes/wp-content/uploads/template-structure.png)
[Get this file for a larger view](https://press.codes/wp-content/uploads/template-structure.png)

## Timber

We use the [Timber](http://wordpress.org/timber-library) plugin to process the twig files, so you can use any of its classes and definitions in your template files.

* [Menus](https://github.com/jarednova/timber/wiki/TimberMenu)
* [Posts](https://github.com/jarednova/timber/wiki/TimberPost)
* [Images](https://github.com/jarednova/timber/wiki/TimberImage)
* [Site](https://github.com/jarednova/timber/wiki/TimberSite)
* [Terms](https://github.com/jarednova/timber/wiki/TimberTerm)
* [Users](https://github.com/jarednova/timber/wiki/TimberUser)
* [Filters](https://github.com/jarednova/timber/wiki/Filters)
