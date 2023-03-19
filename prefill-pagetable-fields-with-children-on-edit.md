---
title: "Prefill PageTable fields with children on edit"
version: 1.0.3
authors:
  - noelboss
tags:
  - pages
  - fields
  - hook
  - init
date: 2018-17-18
---

## Problem

Editing children is not very intuitive right away. The goal is to provide easy ordering and editing of children without loosing the page contect. Using a PageTable field to make this more intuitive, but children are not added automatically to this field.

## Solution

I use a PageTable field to make edits to children of pages more intuitive…

To register the hooks, insert the following Snippet inside your init function in your module (or add it to your init.php file):

```php
/**
 * Initialize the module.
 *
 * ProcessWire calls this when the module is loaded. For 'autoload' modules, this will be called
 * when ProcessWire's API is ready. As a result, this is a good place to attach hooks.
 */
public function init()
{
  // Prefill pageTable field
  $this->wire()->addHookBefore('InputfieldPageTable::render', $this, 'addChildrenToPageTableFieldsHook');
  $this->wire()->addHookBefore('InputfieldPageTableAjax::checkAjax', $this, 'addChildrenToPageTableFieldsHook');
}
```

Then, add this hook method:

```php
/**
* Fill pagetable fields with children before editing….
*
* @param HookEvent $event
*/
public function addChildrenToPageTableFieldsHook(HookEvent $event)
{
$field = $event->object;

// on ajax, the first hook has no fieldname
if (!$field->name) {
  return;
}

// get the edited backend page
$editID = $this->wire('input')->get->int('id');
if (!$editID && $this->wire('process') instanceof WirePageEditor) {
  $editID = $this->wire('process')->getPage()->id;
}
$page = wire('pages')->get($editID);

// disable output formating – without this, the ajax request will not populate the field
$page->of(false);

// you could also insert a check to only do this with sepcific field names…
// $page->set($field->name, $page->children('template=DesiredTemplate')); // just specific templates
$page->set($field->name, $page->children);

}
```

Now whenever there is a page-table field on your page, it gets populated with the children.

---

### Resources

- [See Forum-Post](https://processwire.com/talk/topic/19634-a-hook-to-prefill-pagetable-fields-with-children-on-edit/)
