---
title: "Improve accessibility of a read more link version: 1.0.0"
authors:
  - marcus
tags:
  - accessibility (a11y)
  - templates
date: 2016-02-14
---

## Problem

You want to enhance (screenreader) accessibility on a "read more" link in a teaser scenario like this:

**Image no longer exists**

```md
![Teaser Scenario](https://bigger-on-the-inside.net/site/assets/files/1037/news-demo-v1.png)
```

[W3C's Accessibility Guidelines on links and content](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-refs.html)

## Solution

Add a visually hidden, but audible element like this:

```php
<?php
$news = $pages->find('template=news');

foreach($news as $teaser): ?>

    <article>
        <!-- The title -->
        <h3><a href="<?= $teaser->url ?>"><?= $teaser->title ?></a></h3>

        <!-- The excerpt or summary -->
        <p><?= $teaser->summary ?></p>

        <!-- The link, extended -->
        <a href="<?= $teaser->url ?>">Read more <span class="visually-hidden"> about <?= $teaser->title ?></a>
    </article>

<?php endforeach; ?>
```

Also add to your CSS:

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip-path: rect(0, 0, 0, 0);
  border: 0;
}
```

---

### Resources

- [Blog post on this topic](https://bigger-on-the-inside.net/articles/better-accessibility-with-processwire-read-more-links/)
- [W3C's Accessibility Guidelines on links and content](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-refs.html)
