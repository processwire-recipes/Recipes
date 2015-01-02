title: Typical breadcrumbs example

----

version: 0.0.1

----

authors: dragan

----

tags: template, pages, parent, breadcrumbs

----

problem:
You want to create a typical breadcrumb list, with last item not linked and showing the current page

----

solution:
```PHP
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

----

resources:
* [Dragans ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/breadcrumbs.inc)
