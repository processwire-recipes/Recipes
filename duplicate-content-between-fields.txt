title: Duplicate content from one field to another
----
version: 0.0.1
----
authors: adrian
----
tags: fields, content, copy
----
problem:
You want to change the field settings on a field so they are different for two separate templates which are currently using the same field. So you need to end up with two separate fields: eg. body1 and body2, each with different settings. The problem is that there is already a considerable amount of content populated into the field in both templates.
----
solution:
Use ProcessWire API to migrate the content so it exists in both fields, then you can remove one field from one template and the other field from the other template:

```
// this duplicates the content in fieldname1 into fieldname2 for all pages with the template called: templatename
$source = "fieldname1";
$destination = "fieldname2";
$template = "templatename";

foreach($pages->find("template=$template") as $p){
    $p->$destination = $p->$source;
    $p->of(false);
    $p->save($destination);
}
```
----
resources:
