title: Get random page via API, e.g. from a pool of testimonials

----

version: 0.0.1

----

authors: dragan

----

tags: pages, random, shuffle

----

problem:
You have a pool of pages, e.g. quotes/testimonials, and want to display a random one.

----

solution:
A little example how to pull a random page:
```PHP
$quote = $pages->find("parent=1041, include=hidden"); // example parent

if(count($quote)) {

	$quote->shuffle();
	$pick = $quote->getRandom();
	$q = $pick->quote; // example field
	$a = $pick->author; // example field

	echo "<div class='quoteWrapper'>\n";
	echo "	<div class='quote'>$q</div>\n";
	echo "	<div class='author'>— $a</div>\n";
	echo "</div>\n";

}
```

----

resources:
* [Dragans ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/randomQuote.inc)
