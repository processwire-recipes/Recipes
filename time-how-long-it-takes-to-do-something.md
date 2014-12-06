title: Time how long it takes to do something

----

version: 0.0.1

----

authors: @processwire

----

tags: debug, time, performance, selectors

----

problem:
You don’t know how performant certain code structures or selector usages are.

----

solution:

```PHP
$t = Debug::timer(); 
do_something(); 
echo Debug::timer($t);
```

----

resources:
* [Tweet](https://twitter.com/processwire/status/452439920653893633)
* [Usage examples in forum thread](https://processwire.com/talk/topic/6328-optimizing-code-queries/)
