title: Resetting admin password via API

----

version: 1.0.5

----

authors: Nico Knoll, owzim, mindplay-dk, LostKobrakai, arjen

----

tags: api, users, password, emergency

----

problem:
For some reason, you have managed to lock yourself out of a site you are currently developing.

----

solution:
Paste the following into a file (e.g. "reset.php") in the root folder of the site, then run it.

ProcessWire version >= 2.6.9

```PHP

require "index.php";
$admin = $users->get('admin'); // or whatever your username is
$admin->setAndSave('pass', 'yo123456');
```

ProcessWire version < 2.6.9

```PHP

require "index.php";
$admin = wire('users')->get('admin');
$admin->setOutputFormatting(false);
$admin->set('pass', 'yo12345');
$admin->save('pass');
```

----

resources:
* [Forum thread](https://processwire.com/talk/topic/7167-server-error-with-latest-dev-build/#entry69041)
* [Blog post](https://processwire.com/blog/posts/processwire-2.6.9-core-updates-and-new-procache-version/)
* [API reference](https://processwire.com/api/ref/page/set-and-save/) 
