---
title: "Create image variations automatically"
version: 1.0.0
authors:
  - wbmnfktr
tags:
  - images
  - creation
date: 2023-11-14
---

## Problem

You want to create your very own image variations right after uploading your images to speed up the first load of your page. With this little addition image variations will be created right after page save.

## Solution

```php
// /site/ready.php
$wire->addHookAfter('Pages::saveReady', function($event) {
    $page = $event->arguments(0);
    // modify to your template
    if($page->template == 'recipe') {
        foreach($page->images as $image) {
            // modify to create your needed image dimensions
            // create and save image variations
            $image->size(1200);
            $image->size(640);
            $image->size(320);
        }
    }
});
```
