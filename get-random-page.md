title: Get random page via API, e.g. from a pool of testimonials

----

version: 1.2.0

----

authors: dragan, apeisa, marcus

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

	echo "<blockquote>\n";
	echo "	<p>$q</p>\n";
	echo "	<footer>— <cite>$a</cite></footer>\n";
	echo "</blockquote>\n";

}
```

----

resources:
* [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/randomQuote.inc)
