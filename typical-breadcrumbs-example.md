---
title: "Typical breadcrumbs example"
version: 1.0.1
authors:
  - dragan
tags:
  - breadcrumbs
  - pages
  - parent
  - templates
date: 2018-12-24
---

## Problem

You want to create a typical breadcrumb list, with last item not linked and showing the current page

## Solution

```php
echo "<ul class=\"breadcrumbs \">";

$parents = $page->parents;

foreach($parents as $parent) {
	$url = $parent->url;
	echo "<li><a href='$url'>{$parent->title}</a></li>\n";
}

// show current / "we are here" page as well, but not as link: (last element)
echo "<li>{$page->title}</li>\n";

echo "</ul>";
```

Alternative:

```php
echo "<ul class=\"breadcrumbs \">";

echo $page->parents->each("<li><a href='{url}'>{title}</a></li>\n");

// show current / "we are here" page as well, but not as link: (last element)
echo "<li>{$page->title}</li>\n";

echo "</ul>";
```

---

### Resources

- [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/breadcrumbs.inc)
