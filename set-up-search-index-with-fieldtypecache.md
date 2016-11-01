title: Set up a really simple search index using FieldtypeCache

----

version: 1.0.1

----

authors: teppo

----

tags: cache, search, index

----

problem:
You have a search page – be it your regular site search or something more specific, such as a search feature for contacts list. While you need to perform a text search to multiple fields, you also want to keep things fast.

Searching from multiple fields is easy, but if overused, it can really kill the performance of those search queries. We need a solution that finds content from multiple fields with just one query. In other words: we need a search index.

----

solution:
We're going to use the native Fieldtype Cache to build a rudimentary search index:

1. First of all, go to Modules and install a Core module called FieldtypeCache.
2. Add a new field "search_cache" and select "Cache" as it's type. Save the field.
3. On the Details tab choose the fields you want to make searchable. Save again.
4. Add "search_cache" to all templates you want to include in your search results.
5. Optional but recommended: use the "Regenerate Cache" option found from the Details tab of the field to make existing content instantly searchable.

With the cache field now in place, all you need to do is use it in your search page, just like you would use any other field:
```PHP
$q = $sanitizer->selectorValue($input->get->q);
if ($q) {
    $pages->find('search_cache%=' . $q);
    // ... do something with the results ...
}
```

Please note that if your PHP version is lower than 5.4, FieldtypeCache escapes multibyte Unicode characters as \uXXXX. We can't really recommend using PHP < 5.4, but if you do, this is worth keeping in mind. Apart from that, this is a pretty neat way to notably speed up certain types of queries.

----

resources:
* [ProcessWire Weekly 129](https://weekly.pw/issue/129)