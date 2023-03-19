---
title: "Enhance paginator with rel attribute for SEO"
version: 1.0.0
authors:
  - teppo
  - Philipp
  - ceberlin
tags:
  - seo
  - pagination
date: 2015-03-31
---

## Problem

Search engines want special code in the header to identify pages with a paginator correctly and there is no obvious way of implementing this with the ProcessWire paginator.

```html
<a rel="prev" href="http://www.example.com/article?page=1" />...</a>
<a rel="next" href="http://www.example.com/article?page=3" />...</a>
```

---

## Solution

```php
$limit = 12; // the "limit"-setting used on this page
$children = $page->children("limit=" . $limit);
$totalpages = ceil($children->getTotal() / $limit);

if ($input->pageNum) {
    if ($input->pageNum < $totalpages) {
        echo "<a rel='next' href='" . $page->url . $config->pageNumUrlPrefix . ($input->pageNum + 1) . "' />";
    }
    if ($input->pageNum > 1) {
        echo "<a rel='prev' href='" . $page->url . $config->pageNumUrlPrefix . ($input->pageNum - 1) . "' />";
    }
}

// Output:
foreach($children as $child): ?>

...

<?php endforeach; ?>

<?php echo $children->renderPager(); ?>
```

---

### Resources

- [Forum thread](https://processwire.com/talk/topic/5145-paginator-and-seo/)
