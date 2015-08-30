title: Create custom admin settings page link in admin menu

----

version: 1.0.0

----

authors: jlahijani

----

tags: page, settings, admin

----

problem:
You want to create a link in the admin menu bar that allows you to easily access a global settings page for your site, without having to create a custom process module or going to the page using the pagetree.

----

solution:
 
Let's say the page name of your settings page is called "settings" and it's directly under home.
 
Now, create a *new* page under the admin page in ProcessWire.  Use the "admin" template.  Call this page "settings" as well.  After saving, it will now give you the option to choose which process module this admin page will utilize.  Choose "ProcessPageEdit".
 
Now you will have a link in the main navbar to this Settings page, but when you go to it, it will error saying "Unknown page" + "the process returned no content".
 
What you need to do is make this page load the /settings/ page you originally made.
 
Using the special "/site/ready.php" file (create ready.php if it doesn't exist), you can add the following line of code to it:

```PHP
// inside /site/ready.php
if($page->template=="admin" && $page->name=="settings") $input->get->id = $pages->get("/settings/")->id;
```

Now go to the Settings page in the navbar.  Nice, easy and "official" feeling.

----

resources:
* [How to create a custom admin settings page](https://processwire.com/talk/topic/6388-how-to-create-a-custom-admin-settings-page/?p=99829)
