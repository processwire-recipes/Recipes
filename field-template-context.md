---
title: "Field/template context"
version: 1.0.0
authors:
  - owzim
  - kongondo
tags:
  - api
  - templates
  - fields
  - context
date: 2015-01-17
---

## Problem

Fields can have template-contextual settings and as of ProcessWire [2.5.7](http://processwire.com/blog/posts/processwire-2.5.7-core-updates/) you can set _all_ field settings in a template context. Using the [API](https://processwire.com/api/) it works a bit differently than the usual `$f->set('setting', $value); $f->save();`.

## Solution

```php
// get the template
$t = $templates->get('basic-page');

// get the field in context of this template
$f = $t->fieldgroup->getField('summary', $useFieldgroupContext = true);

// value of the field
$f->description = "Description of 'summary' only in context of 'basic-page'";

// save new setting in context
$fields->saveFieldgroupContext($f, $t->fieldgroup);
```

---

### Resources

- Forum thread: [Change field description in context via API](https://processwire.com/talk/topic/6656-change-field-description-in-context-via-api/?p=65139)
- Blog post: [Field/template context now available for any field property](http://processwire.com/blog/posts/processwire-2.5.7-core-updates/#field-template-context-now-available-for-any-field-property)
