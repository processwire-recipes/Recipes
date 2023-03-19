---
title: "Converting thumbnail images for PW3 admin"
version: 1.0.1
authors:
  - ryan
tags:
  - processwire3
  - admin
  - thumbnails
date: 2016-11-02
---

## Problem

Lets say that you've recently upgraded to ProcessWire 3.x and noticed that all of your thumbnail images are low-quality in the admin.

## Solution

This is because PW 2.7 used 100-pixel height thumbnails, while PW 3.x uses 260 pixel width thumbnails, quite a size difference. When in the page editor, you'll notice those images look pretty low quality, and that PW gives you a little checkbox you can check to force it to re-create them. But if you've got dozens, hundreds or thousands of pages, you probably aren't going to bother. Here's a way that you can force it to re-create automatically. In your /site/config.php file, add the following:

```php
$config->adminThumbOptions('height', 202);
```

The "202" part actually can be any number, so long as it's not the previous default value of "100", and doesn't overlap with any existing thumbnail sizes (which is why we chose "202" rather than "200", though either would likely work). This "height" property is deprecated and now only used to detect if thumbnails are old or new. So by giving it a value of "202", ProcessWire checks to find no existing thumbnail is present (with height of "202") and it creates a new one using the new default setting of 260px width. The reason ProcessWire uses the old thumbnails (upscaled in the admin) by default is that creating thumbnails can be an expensive process, especially on pages with lots of large images. One of the sites I work on has thousands of pages with dozens of multi-megabyte villa photos on each. Creating all those thumbnails anew could really slow the editors down when they go to edit each page. They don't care about blurry thumbnails, but they would really care about being slowed down when making edits to existing pages. So before you implement this recipe, make sure your context is not one that's going to result in calls from the client about why the server is slow. :)

---

### Resources

- [Ryan's post on processwire.com](https://processwire.com/blog/posts/processwire-3.0.39-core-updates/)
