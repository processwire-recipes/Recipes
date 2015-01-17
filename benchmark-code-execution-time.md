title: Benchmark code execution time

----

version: 1.0.0

----

authors: ryan

----

tags: debug, time, performance

----

problem:
You don’t know how performant certain code structures or selector usages are.

----

solution:

```PHP
$t = Debug::timer();
// do some expensive code execution here
echo Debug::timer($t);
```

----

resources:
* [Tweet](https://twitter.com/processwire/status/452439920653893633)
* [Usage examples in forum thread](https://processwire.com/talk/topic/6328-optimizing-code-queries/)
