title: Find latest modified (sub)child of a page

----

version: 0.0.1

----

authors: @isellsoap

----

tags: pages, modified, latest, children

----

problem:
A page has got a possibly deeply nested children structure and you want to output the latest modified child of that page.

----

solution:

```PHP
$page->find("id>0,sort=-modified")->first
```

----

resources:
* [Tweet](https://twitter.com/isellsoap/status/361471127379378177)
