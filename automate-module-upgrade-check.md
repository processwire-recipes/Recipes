---
title: "Automatically check for module updates (get notified by mail) [WIP]"
version: 1.0.0
authors:
  - monollonom
  - wbmnfktr
tags:
  - lazycron
  - modules
  - updates
  - automation
date: 2023-05-14
---

## Problem

Your use case may differ, but some people appreciate receiving notifications about available updates for installed modules. This was recently requested in the **Module: ProcessWire Core Upgrade** support thread and was promptly replied by the amazing ProcessWire community.

---

## Solution

We use LazyCron every four weeks to see whether there are any upgrades available using the ProcessWire Core Upgrade module.

```php
wire()->addHookAfter("LazyCron::every4Weeks", function(HookEvent $event) {
    $checker = $event->modules->get("ProcessWireUpgradeCheck");
    if(!$checker) return;

    $upgrades = $checker->getModuleVersions(true); // we only want modules with new versions
    if(!count($upgrades)) return;

    $subject = "There are " . count($upgrades) . " modules to update";

    $body = "Hi!\n\nAn upgrade is available for these modules on " . wire('config')->httpHost . ":\n\n";

    foreach($upgrades as $name => $info) {
        $body .= "- $name ($info[remote])\n";
    }

    $body .= "\nHead to " . $event->pages->get("process=ProcessWireUpgrade")->httpUrl . " to upgrade."; // not sure if this `get` would work?

    $mail = wireMail();
    $mail->from("wwwuser@example.com")
        ->to("admin@example.com")
        ->subject($subject)
        ->body($body)
        ->send();
});
```

More explanations and updates will be provided in the near future.

---

## Resources

- https://processwire.com/talk/topic/7525-module-processwire-core-upgrade/
- https://processwire.com/talk/topic/7525-module-processwire-core-upgrade/page/6/#comment-232755
- https://processwire.com/modules/process-wire-upgrade/
- https://processwire.com/docs/more/lazy-cron/
