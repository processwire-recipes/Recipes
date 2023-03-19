---
title: "Get unformatted field values"
version: 1.0.1
authors:
  - SiNNuT
  - marcus
tags:
  - fields
  - unformatted
date: 2015-01-17
---

## Problem

You need to return a field value unformatted in order to format it individually later.

## Solution

You can access the naked field value with the getUnformatted method like so:

```php
<?php echo date("F j, Y, g:i a", $page->getUnformatted("a_date_field")); ?>
```

---

### Resources

- [Forum thread](https://processwire.com/talk/topic/1978-how-to-format-dates-in-templates/#entry18517)
