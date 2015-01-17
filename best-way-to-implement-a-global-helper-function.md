title: Best way to implement a global utility function

----

version: 1.0.0

----

authors: horst

----

tags: config, autoload, helpers

----

problem: 
Some other frameworks and CMS's you have used have - for example - a handy function called "h", which is a shortcut to htmlspecialchars($string, ENT_QUOTES, 'utf-8'). This makes it super-easy to escape output in your templates. In ProcessWire, there is by default no such helper present by default.

----

solution:
Using own utility function files in my site-profiles in a subfolder called, for example: 'mylibs'
 
How to include them once in the site/config.php file:  

```PHP
include_once(dirname(__FILE__) . "/mylibs/myfunctions.php");
```

This way you can use all what the features/methods this utility files everywhere.

----

resources:
* [Forum thread](https://processwire.com/talk/topic/7573-best-way-to-implement-a-global-utility-function/#entry73157)
