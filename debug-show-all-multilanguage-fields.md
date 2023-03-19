---
title: "Debug: Show all template field names and labels (for each language)"
version: "1.0.1"
authors:
  - dragan
tags:
  - pages
  - multilanguage
  - fields
date: 2015-01-17
---

## Problem

You need an overview of all template fields and labels in every language in your setup

## Solution

If you want to activate all languages, use this after your page-creation API script. In this real-life scenario I had to import product pages, hence the template-selector below.

```php
// debug / info script:
// show all template field names and labels (for each language)
// proved to be essential to avoid chaos with almost 100 template fields in a product template

$template = $templates->get('product'); // example template

echo "<table border=1 cellpadding=3 cellspacing=0>";
  echo "<tr>";
  echo "<th>Field Name</th>";
  echo "<th>Field Label DE</th>"; // example languages
  echo "<th>Field Label EN</th>";
  echo "<th>Field Label US</th>";
  echo "<th>Field Label FR</th>";
  echo "</tr>";

foreach($template->fields as $field) {
  echo "<tr>";
  echo "<td>" . $field->name . "</td>";
  $user->language = $languages->get("default");
  echo "<td>" . $fields->getLabel($field->name) . "</td>";
  $user->language = $languages->get("en");
  echo "<td>" . $fields->getLabel($field->name) . "</td>";
  $user->language = $languages->get("us");
  echo "<td>" . $fields->getLabel($field->name) . "</td>";
  $user->language = $languages->get("fr");
  echo "<td>" . $fields->getLabel($field->name) . "</td>";
  echo "</tr>";
}
echo "</table>";
```

---

### Resources

- [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/showAllMultilangFields.php)
