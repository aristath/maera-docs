# Templating

Maera is a WordPress theme and as such it follows the default WordPress [template hierarchy](http://wphierarchy.com/). However, we do have some extra stuff going on under the hood that we have found make life a lot easier both for skilled developers and for novice users that may not be php developers.

In addition to the default template files that you can use if you want, we also provide a "views" folder that contains "twig" files. You can learn more about the twig language by clicking on [this link](http://twig.sensiolabs.org/).
`twig` files have a structure and syntax a lot easier and more user-friendly than php. For example if you wanted to echo something in PHP, you would do this:

```php
<?php echo $foo; ?>
```

In twig however it's a lot simpler than that:

```twig
{{ foo }}
```

You can learn more about the syntax and naming conventions by reading the [Timber Docs](https://github.com/jarednova/timber/wiki).

To add your own templates you can create a [Child Theme](href="http://codex.wordpress.org/Child_Themes) and either create custom php template files like you would do on every other WordPress theme, or custom twig templates.
The template hierarchy of the twig files is the same as the [default WordPress templates structure](href="http://wphierarchy.com/), simply by replacing the `.php` suffix with `.twig`.
You can keep your `.twig` files in the root of your child theme, or a `/views` folder if you want to keep things a little more organized.

---

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

## Post properties in twig files

```twig
{{ post.ID }}                 // ID of the post
{{ post.post_author }}        // ID of the post author
{{ post.post_date }}          // timestamp in local time
{{ post.post_date_gmt }}      // timestamp in gmt time
{{ post.post_content }}       // Full (unprocessed) body of the post
{{ post.post_title }}         // title of the post
{{ post.post_excerpt }}       // excerpt field of the post, caption if attachment
{{ post.post_status }}        // the post status
{{ post.comment_status }}     // comment status: open, closed
{{ post.ping_status }}        // ping/trackback status
{{ post.post_password }}      // password of the post
{{ post.post_name }}          // post slug, string to use in the URL
{{ post.post_modified }}      // timestamp in local time
{{ post.post_modified_gmt }}  // timestatmp in gmt time
{{ post.post_parent }}        // id of the parent post.
{{ post.guid }}               // global unique id of the post
{{ post.menu_order }}         // menu order
{{ post.post_type }}          // type of post: post, page, attachment, or custom string
{{ post.post_mime_type }}     // mime type for attachment posts
{{ post.comment_count }}      // number of comments
{{ post.terms }}              // taxonomy terms
{{ post.custom_field }}       // custom fields (replace custom_field with your own)
```

You can use post properties, fields and content in your `.twig` template files however you want.
On the right you can see what is available and what it does.
