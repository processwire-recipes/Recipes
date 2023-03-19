---
title: "Create breadcrumb of page titles for html title tag"
version: 1.0.0
authors:
  - Tom Arnold
tags:
  - title
  - page title
  - tag
  - html
date: 2016-02-13
---

## Problem

You want to have a html page title that shows the rootline to the current page, for example "Hot News « Articles « Cool Website", if your page tree looks like this:

|- Home (Cool Website)

|-- Articles

|--- Hot News

---

## Solution

Put this in your \_func.php or where ever your setup has a place for shared functions

```php
function renderPageTitle($options=array()){
	$defaults = array(
		'glue'    => ' « ', // ' » '
		'reverse' => true   // false
	);
	$opts = array_merge($defaults,$options);
	$page = wire('page');
	$parents = $page->parents;
	$items = array();
	foreach($parents as $parent) {
		$items[] = $parent->title;
	}
	$items[] = $page->title;
	if($opts['reverse']){
		$items = array_reverse($items);
	}
	$out = implode($opts['glue'],$items);
	return $out;
}
```

and then use it in your template like this:

```html
<?php $pageTitle = renderPageTitle(); ?>
...
<title><?php echo $pageTitle; ?></title>
```

This will list the current page first, and every parent page up to the root/home page after, separated with a "«". If you want to reverse the the order, there is an option array parameter, where the order and the "glue", the char that's shown as a separator between the page titles, can be specified.

```html
<?php
$options = array(
	'reverse' =>
false, 'glue' => ' | ' ); $pageTitle = renderPageTitle($options); ?> ...
<title><?php echo $pageTitle; ?></title>
```

this will print out "Cool Website | Articles | Hot News".

---

### Resources
