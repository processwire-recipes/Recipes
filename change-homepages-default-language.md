---
title: "Change homepage's default language"
version: 1.0.1
authors:
  - bcartier
  - ESRCH
tags:
  - multilanguage
  - homepage
  - modules
  - hook
date: 2015-03-31
---

## Problem

You have a multi-language site, and started with for example English as primary, and French as secondary language (so the English homepage is `example.org/`, the French one `example.org/fr`). Later in the project you want to set French as primary.

## Solution

Here is what you can do to redirect the home page '/' to the French home page:

- Set page names for both languages for the home page ('en' for English, 'fr' for French)
- In the LanguageSupportPageNames settings, choose the option "No - Root URL performs a redirect to: /name/". When you go to the root url '/' it will redirect you to '/en/'
- Finally, **create a module** to hook into Session::redirect to force the redirection of the root url to the French translation as follows:

```php
<?php

	class LanguageDefault extends WireData implements Module {

		/**
		 * getModuleInfo is a module required by all modules to tell ProcessWire about them
		 *
		 * @return array
		 *
		 */
		public static function getModuleInfo() {

			return array(
				'title' => 'LanguageDefault',
				'version' => 1,
				'summary' => 'A work around to changing the default language.',
				'href' => 'https://processwire.com/talk/topic/9322-change-default-language-for-homepage/?p=89717',
				'singular' => true,
				'autoload' => true,
			);
		}

		/**
		 * Initialize the module
		 *
		 * ProcessWire calls this when the module is loaded. For 'autoload' modules, this will be called
		 * when ProcessWire's API is ready. As a result, this is a good place to attach hooks.
		 *
		 */
		public function init() {
			$this->session->addHookBefore('redirect', $this, 'setDefaultLanguage');
			// The hook checks whether you are viewing the home page, and whether you are redirecting to the English url, and if so, it changes the url to the French url
		}


		public function setDefaultLanguage($event) {
			if ($this->page->id == 1 && $event->arguments(0) == $this->page->localUrl('default')) {
		      $event->arguments(0, $this->page->localUrl('fr'));
		    }
		}

	}
```

---

### Resources

- [bcartiers's forum post](https://processwire.com/talk/topic/9322-change-default-language-for-homepage/#entry89925)
