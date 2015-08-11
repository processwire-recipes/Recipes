title: Quick ProcessWire first child redirect snippet

----

version: 1.0.4

----

authors: marcus, dsdsdsds

----

tags: session, templates, structure

----

problem:
From time to time it happens on a website project that you don't really have pages with teaser- or distributing-functions, but are in need for a first child redirect.

----

solution:
Use ProcessWire API to redirect:

```PHP
<?php
/**
 * Template: First Child redirect
 *
 */

// if current page has children (published, not hidden), redirect (HTTP 302) to its first child
if($pages->count("parent=$page")) $session->redirect($page->child->url, false);
```

----

resources:
