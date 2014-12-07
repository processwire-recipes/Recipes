title: Resetting admin password via API

----

version: 0.0.2

----

authors: Nico Knoll

----

tags: api, users, password, emergency

----

problem: 
For some reason, you have managed to lock yourself out of a site you are currently developing.

----

solution:
```PHP
$users->get("admin")->setOutputFormatting(false)->set('pass', 'yo12345')->save();
```

----

resources:
* [Forum thread](https://processwire.com/talk/topic/7167-server-error-with-latest-dev-build/#entry69041)
