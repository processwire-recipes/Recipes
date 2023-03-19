---
title: "Logging all outgoing e-mails"
version: 1.0.0
authors:
  - ryan
tags:
  - email
  - log
  - hook
  - ready
date: 2016-10-30
---

## Problem

Lets say that you want to maintain a log of every email sent from your site.

## Solution

This can be done quite easily by hooking the WireMail class. Place the following in your /site/ready.php file:

```php
$wire->addHookAfter('WireMail::send', function($event) {
  $mail = $event->object;
  $event->wire('log')->save('sent-mail',
    "SENT ($event->return of " . count($mail->to) . "), " .
    "SUBJECT ($mail->subject), " .
    "TO (" .  implode(", ", $mail->to) . "), " .
    "FROM ($mail->from)"
  );
});
```

Test things out by sending an email. Here's an example of how I might send a message with the API:

```php
$mail->new()
  ->to('test@processwire.com')
  ->from('ryan@test.com')
  ->subject('Hello world')
  ->body('How are you doing?')
  ->send();
```

In your admin, if you go to Setup > Logs > sent-mail, you should see a log entry like this:

```
SENT (1 of 1), SUBJECT (Hello world), TO (test@processwire.com), FROM (ryan@test.com)
```

---

### Resources

- [Ryan's post on processwire.com](https://processwire.com/blog/posts/processwire-3.0.38-core-updates/#recipe-logging-all-outgoing-emails)
