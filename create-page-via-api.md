title: Create page via API

----

version: 1.0.1

----

authors: dragan

----

tags: page, creation

----

problem:
You want to create one or more pages, bypassing the admin interface

----

solution:

```PHP
$p = new Page();
$p->setOutputFormatting(false);
$p->template = 'products'; // example template
$p->parent = wire('pages')->get('/'); // example parent
$p->name = "foo"; // example name
$p->title = "My API-generated new PW page";
$p->fieldname = "my field value"; // example field with example value
$p->save();
echo "page ID {$p->id} created!<br>";
```

----

resources:
* [Dragan's ProcessWire code snippets](https://github.com/dragan1700/pw/blob/master/createPage.php)
