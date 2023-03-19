---
title: "Quick ProcessWire first child redirect snippet"
version: 1.0.4
authors:
  - marcus
  - dsdsdsds
tags:
  - session
  - templates
  - structure
date: 2015-08-11
---

## Problem

From time to time it happens on a website project that you don't really have pages with teaser- or distributing-functions, but are in need for a first child redirect.

## Solution

Use ProcessWire API to redirect:

```php
<?php
/**
 * Template: First Child redirect
 *
 */

// if current page has children (published, not hidden), redirect (HTTP 302) to its first child
if($pages->count("parent=$page")) $session->redirect($page->child->url, false);
```

---

### Resources
