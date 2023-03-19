---
title: "Use different sets of template files, e.g. based on hostname"
version: 1.0.0
authors:
  - Raymond Geerts
tags:
  - url
  - templates
  - switches
date: 2015-01-17
---

## Problem

You want to use a different location than `site/templates` when storing your template files - or use different sets of template files in parallel, based on a certain switch.

## Solution

You are able to overrule this setting from the site/config.php as follows

```php
$config->urls->templates = $config->urls->site . 'mytemplates/';
$config->paths->templates = $config->paths->site . 'mytemplates/';
```

A trick one can use when developing in a live site is to duplicate the template folder and rename the copy to templates-dev. Then make sure have an other domain (or subdomain) pointing to the hosting, for example **dev.domain.ext** and for the live site on **www.domain.ext**.

With the following code you can access the development templates while normal visitors keep getting the default templates.

```php
/**
* Development example: Alias for template-dev access
*
*/
if($_SERVER['HTTP_HOST'] == 'dev.domain.ext') {
    $config->urls->templates = $config->urls->site . 'templates-dev/';
    $config->paths->templates = $config->paths->site . 'templates-dev/';
    $config->debug = true;
}
```

You can make up all kind of 'rules'/conditions to go by when overruling the templates folder with php from the config.php file.
For example serving the same site with different layout and code for mobile on **m.domain.ext**:

```php
/**
* Mobile example: Alias for template-mobile access
*
*/
if($_SERVER['HTTP_HOST'] == 'm.domain.ext') {
    $config->urls->templates = $config->urls->site . 'templates-mobile/';
    $config->paths->templates = $config->paths->site . 'templates-mobile/';
}
```

---

### Resources

- [Forum post](https://processwire.com/talk/topic/8789-should-all-template-files-put-under-sitetemplates-folder/#entry84861)
