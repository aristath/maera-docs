# Actions

## Generic actions

`maera/admin/save`

This action is run when the admin page is saved.
An example usage is when we import the customizer options in a shell that uses a compiler. In that case, when we perform the import we may want to trigger the shell's compiler so that the stylesheets get re-generated, caches dumped etc.

`maera/shell/include_modules`

This action is run when the shell is being initialized.

## Content-injecting actions

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
