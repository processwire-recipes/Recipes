---
title: "Get random page via API, e.g. from a pool of testimonials"
version: 1.2.0
authors:
  - apeisa
  - dragan
  - marcus
tags:
  - pages
  - random
  - shuffle
date: 2016-10-30
---

## Problem

You have a pool of pages, e.g. quotes/testimonials, and want to display a random one.

## Solution

A little example how to pull a random page:

```php
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

---

### Resources

- [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/randomQuote.inc)
