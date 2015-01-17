title: Get unformatted field values

----

version: 1.0.1

----

authors: SiNNuT, marcus

----

tags: fields, unformatted

----

problem:
You need to return a field value unformatted in order to format it individually later.

----

solution:
You can access the naked field value with the getUnformatted method like so:
```PHP
<?php echo date("F j, Y, g:i a", $page->getUnformatted("a_date_field")); ?>
```

----

resources:
* [Forum thread](https://processwire.com/talk/topic/1978-how-to-format-dates-in-templates/#entry18517)
