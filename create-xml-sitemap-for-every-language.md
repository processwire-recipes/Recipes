title: Create an XML sitemap for every language

----

version: 1.0.2

----

authors: dragan

----

tags: pages, multilanguage, xml-sitemap, seo

----

problem:
With PW multilanguage setup, you'd have normally to manually "glue" multilanguage sitemaps XMLs together.

----

solution:
```PHP
/*
Google et al usually only accepts one single XML, not one for each language
With PW multilang. setup, you'd have normally to manually "glue" these XMLs together
e.g. site.com/en/sitemap.xml + site.com/de/sitemap.xml + site.com/fr/sitemap.xml etc.
Functions taken from Ryan Cramer:
http://processwire.com/talk/topic/3846-how-do-i-create-a-sitemapxml/?p=37613
*/
// opening XML tag + node:
$sitemapCollection = '<?xml version="1.0" encoding="UTF-8"?>' . "\n" .
		'<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">';
		
function renderSitemapPage(Page $page) {
	return 	"\n<url>" .
		"\n\t<loc>" . $page->httpUrl . "</loc>" .
		"\n\t<lastmod>" . date("Y-m-d", $page->modified) . "</lastmod>" .
		"\n</url>";
}

function renderSitemapChildren(Page $page) {
	$out = '';
	$newParents = new PageArray();
	$children = $page->children;
	foreach($children as $child) {
		$out .= renderSitemapPage($child);
		if($child->numChildren) $newParents->add($child);
			else wire('pages')->uncache($child);
	}
	foreach($newParents as $newParent) {
		$out .= renderSitemapChildren($newParent);
		wire('pages')->uncache($newParent);
	}
	return $out;
}

function renderSitemapXML(array $paths = array()) {
	$out = '';
	array_unshift($paths, '/'); // prepend homepage
	foreach($paths as $path) {
		$page = wire('pages')->get($path);
		if(!$page->id) continue;
		$out .= renderSitemapPage($page);
		if($page->numChildren) $out .= renderSitemapChildren($page);
	}
	return $out;
}

// DE: (default lang. here)
$user->language = $languages->get("default");
$sitemapCollection .= renderSitemapXML();
$pgs = $pages->find("template=product, include=hidden, sort=sort");
foreach ($pgs as $p) {
	$sitemapCollection .= 	"\n<url>" .
		"\n\t<loc>" . $p->httpUrl . "</loc>" .
		"\n\t<lastmod>" . date("Y-m-d", $p->modified) . "</lastmod>" .
		"\n</url>";
}

// EN:
$user->language = $languages->get("en");
$sitemapCollection .= renderSitemapXML();
$pgs = $pages->find("template=product, include=hidden, sort=sort");
foreach ($pgs as $p) {
	$sitemapCollection .= 	"\n<url>" .
		"\n\t<loc>" . $p->httpUrl . "</loc>" .
		"\n\t<lastmod>" . date("Y-m-d", $p->modified) . "</lastmod>" .
		"\n</url>";
}

// etc. - repeat for each language
// close XML node

$sitemapCollection .= "\n</urlset>";

header("Content-Type: text/xml");
echo $sitemapCollection;

```

----

resources:
* [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/xmlSitemapMultilang.php)
