---
title: "Hook Page Publish"
version: 1.0.1
authors:
  - webmanufaktur
tags:
  - hook
date: 2024-05-10
---

## Problem

You need to return a field value unformatted in order to format it individually later.

## Solution

You can access the naked field value with the getUnformatted method like so:

```php
$this->addHookAfter('Pages::published', function (HookEvent $event) {

  $pages = $event->object;
  $page = $event->arguments(0);

  // add conditionals here

  wire()->log->save("debug", "page was published");
});
```

---

### Resources

- [Forum thread](https://processwire.com/talk/topic/1978-how-to-format-dates-in-templates/#entry18517)
