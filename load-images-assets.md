---
title: "Load Remote Assets On-Demand from another ProcessWire Instance"
version: 1.0.0
authors:
  - ryan
tags:
  - processwire
  - dev
date: 2023-05-18
---

## Problem

You are working on a local, remote or any other copy of your live site and only have the basic files without any assets (normally saved in **_/site/assets/files/_**) - but you maybe need some of the files. This **hook** does this for you. In case any of your pages, either frontend or backend, need a linked file it will download a copy to your very instance of your project.

## Solution

Place this in your **_/site/ready.php_**, change the **_domain.tld_** entry to your real live instance and you are already done with this enormous timesaver. Keep in mind that this only handles ProcessWire-instances and nothing like _Processwire-2-Joomla_ or _ProcessWire-2-WordPress_ matchings.

```php
$wire->addHookAfter('Pagefile::url, Pagefile::filename', function($event) {

  $config = $event->wire('config');
  $file = $event->return;

  if($event->method == 'url') {
    // convert url to disk path
    $file = $config->paths->root . substr($file, strlen($config->urls->root));
  }

  if(!file_exists($file)) {
    // download file from source if it doesn't exist here
    $src = 'https://domain.tld/site/assets/files/';
    $url = str_replace($config->paths->files, $src, $file);
    $http = new WireHttp();
    $http->download($url, $file);
  }
});
```

## Resources

- https://processwire.com/blog/posts/pw-3.0.137/#on-demand-mirroring-of-remote-web-server-files-to-your-dev-environment
