title: Maintenance Mode

----

version: 1.0.3

----

authors: owzim

----

tags: admin, maintenance, session

----

problem:
You are currently reworking the page/field/template/module structure of your site
and have a local db dump of the live site on your system but you don't want the
user to make changes in the meantime. A restore from local back to live would
otherwise overwrite the user's changes.

----

solution:
Add this on top of your `/site/templates/admin.php`

```PHP
// check if the user is logged in and if they are not a super user
if ($user->isLoggedIn() && $config->maintenance === true && !$user->isSuperuser()) {
	// logout the user
	$session->logout();
	// spit out an error message via session, so it still appears after the redirect
	$session->error('Database currently in maintenance - logged out');
	// redirect to the login page
	$session->redirect($config->urls->admin);
}
```

In your `/site/config.php` you add a config value `maintenance` and change it back
to `false` if you're done maintaining.


```PHP
$config->maintenance = true;
```

----

resources:
