---
title: "Get all unused fields via API"
version: 1.0.0
authors:
  - gebeer
tags:
  - fields
  - API
date: 2023-04-19
---

## Problem

You want to be a good housekeeper and do some cleanup on a complex site with many fields and templates. After years of working on the project you have lost track of which fields are actually used by any templates.

## Solution

Don't worry and get all unused fields via API. The Field class has a method `getTemplates()` that we can use. This method returns all templates that have this field assigned. If the count of `getTemplates()` is 0 then we can be sure that this field is not used by any template.

```php
$danglingFields = new WireArray();
foreach($fields as $f) if($f->getTemplates()->count === 0) $danglingFields->add($fields->get($f));
// now $danglingFields is a WireArray of Field objects. Do with it as you please.
// You could delete all unused fields:
// foreach($danglingFields as $f) $fields->delete($f);
// But caution. They will be gone for good. You better make a DB backup before doing this.
```
---

### Resources


