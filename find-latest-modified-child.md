---
title: "Find latest modified (sub)child of a page"
version: 1.0.0
authors:
  - isellsoap
tags:
  - pages
  - modified
  - latest
  - children
date: 2015-01-17
---

## Problem

A page has got a possibly deeply nested children structure and you want to get the latest modified child of that page.

## Solution

```php
$lastModifiedChild = $page->find("id>0,sort=-modified")->first;
```

---

### Resources

- [Tweet](https://twitter.com/isellsoap/status/361471127379378177)
