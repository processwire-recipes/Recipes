title: Quick ProcessWire first child redirect snippet

----

version: 0.0.1

----

authors: marcus

----

tags: session, template, structure

----

problem:
From time to time it happens on a website project that you don't really have pages with teaser- or distributing-functions, but are in need for a first child redirect.

----

solution:
Use ProcessWire API to redirect:

```
/**
 * Template: First Child redirect
 *
 */

<?php
if($page->numChildren) $session->redirect($page->child()->url);
```

----

resources:
