---
title: "Set language-specific input field labels via API"
version: 1.0.1
authors:
  - dragan
tags:
  - pages
  - multilanguage
  - labels
date: 2015-01-17
---

## Problem

You have a multi-language setup and want to set language-specific labels, bypassing admin interface

## Solution

If you want to activate all languages, use this after your page-creation API script. In this real-life scenario I had to import product pages, hence the template-selector below.

```php
// Assign API variables to make things a little easier
$fields = wire("fields");
$languages = wire("languages");

// Example languages
$en = $languages->get('en');
$us = $languages->get('us');
$fr = $languages->get('fr');

// Example multi-language field
$field = $fields->get("prod_max_power_consumption");
$field->set("label$en", "Max. Power Consumption");
$field->set("label$us", "Max. Power Consumption");
$field->set("label$fr", "Max. consommation de courant");

$field->save();
```

---

### Resources

- [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/setFieldLabels.php)
- [ProcessWire API documentation and getting and setting](http://processwire.com/api/multi-language-support/multi-language-fields/#getting-and-setting)
