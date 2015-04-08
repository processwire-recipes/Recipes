title: Resetting admin password via API

----

version: 1.0.2

----

authors: Nico Knoll, owzim

----

tags: api, users, password, emergency

----

problem:
For some reason, you have managed to lock yourself out of a site you are currently developing.

----

solution:
Paste the following into a file (e.g. "reset.php") in the root folder of the site, then run it.

```PHP
require "index.php";

$admin = wire('users')->get("admin");
$admin->setOutputFormatting(false);
$admin->set('pass', 'yo12345');
$admin->save();
```

----

resources:
* [Forum thread](https://processwire.com/talk/topic/7167-server-error-with-latest-dev-build/#entry69041)
