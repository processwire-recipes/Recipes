title: Resetting admin password via API

----

version: 1.0.3

----

authors: Nico Knoll, owzim, LostKobrakai

----

tags: api, users, password, emergency

----

problem:
For some reason, you have managed to lock yourself out of a site you are currently developing.

----

solution:
```PHP
// concise one liner
$users->get("admin")->of(false)->set('pass', 'yo12345')->save('pass');

// more verbose version
$admin = $users->get("admin");
$admin->setOutputFormatting(false);
$admin->set('pass', 'yo12345');
$admin->save('pass');
```

----

resources:
* [Forum thread](https://processwire.com/talk/topic/7167-server-error-with-latest-dev-build/#entry69041)
