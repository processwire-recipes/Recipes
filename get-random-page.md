title: Get random page via API, e.g. from a pool of testimonials

----

version: 0.0.4

----

authors: dragan, apeisa

----

tags: pages, random, shuffle

----

problem:
You have a pool of pages, e.g. quotes/testimonials, and want to display a random one.

----

solution:
A little example how to pull a random page:
```PHP
$quote = $pages->find("template=quote, include=hidden, sort=random, limit=1")->first();

if($quote->id) {

	$q = $quote->quote; // example field
	$a = $quote->author; // example field

	echo "<div class='quoteWrapper'>\n";
	echo "	<div class='quote'>$q</div>\n";
	echo "	<div class='author'>— $a</div>\n";
	echo "</div>\n";

}
```

----

resources:
* [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/randomQuote.inc)
