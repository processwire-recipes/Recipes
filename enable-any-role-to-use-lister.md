title: Enable any Role to use Lister (pages/find)

----

version: 1.0.0

----

authors: rajo

----

tags: admin, users

----

problem:
You want roles that are not superuser to still be able to use the Pages/Find (lister) feature.  This is not available by default.

----

solution:
In retrospect it seems obvious, but it took me forever to figure it out.

Simply check the page-lister permission to the role in question.

----

resources:
