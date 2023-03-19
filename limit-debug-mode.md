---
title: "Limiting debug mode to development environment"
version: 1.0.2
authors:
  - marcus
tags:
  - debug
  - config
date: 2015-01-17
---

## Problem

Often times, one forgets to switch off ProcessWire's debug mode before deploying to the live environment

## Solution

Here's a Laravel-inspired mini tutorial to prevent that - in this case utilizing MAMP Pro 3:

#### Step 1: Set an environment variable

When managing/creating your virtual host, click the "Extended" tab and fill in _Additional parameters for Virtual Host_:

```http
SetEnv ENV development
```

Doing this we set an environment variable called ENV to a value of "development", for example:

#### Step 2: Slight modification of /site/config.php

Within aforementioned virtual host...

```php
$_ENV["ENV"]
```

...now returns "development". We can use this to add a small switch to ProcessWire's main debug setting:

```php
$config->debug = ($_ENV["ENV"] == "development") ? true : false;
```

This is just a concise way of saying: if $\_ENV["ENV"] is "development", turn debug on (true), else leave it off (false).

As long as your production server has not set an environment variable of this name AND filled it with development, which would be kind of crazy, you're able use debug mode - and free to forget about it.

---

### Resources
