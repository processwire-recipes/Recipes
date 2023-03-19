---
title: "Cache the result of a function"
version: 1.0.0
authors:
  - owzim
tags:
  - cache
  - performance
date: 2016-11-16
---

## Problem

In your modules (or somewhere else) sometimes you have expensive calculations to make, or you simply want to cache a result of a method/function, when its parameters/data do not change on runtime and it’s still accessed multiple times.

**Note:** This a _runtime cache_, nothing is saved to disk or the database.

---

## Solution

```php
function expensiveStuff($name, $someOtherParam, $forceNew = false)
{
    // This static $cache is scoped to the function and will be created only
    // once despite called multiple times
    static $cache = null;
    if (is_null($cache)) $cache = array();

    // Generate a unique ID here, this is just an example
    $id = "{$name}-{$someOtherParam}";

    // Return the cached result if it exists _and_ it should not be recalculated
    if (!$forceNew && array_key_exists($id, $cache)) return $cache[$id];

    $result = '... expensive code here ...';

    // Save the result in the cache and return it
    return $cache[$id] = $result;
}
```

---

### Resources
