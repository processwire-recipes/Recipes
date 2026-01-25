---

title: "Swap the content of multilingual fields between two languages"

---

version: 1.0.0

---

authors: Olaf Gleba

---

tags: pages, languages, multilingual, multilanguages

---

date: 2026-01-25

---



## Problem

On Launch the defined default language of a website is e.g. `german` (`/`). Beside e.g. `english` as a second language (`/en/`). After a while eventually the client wants to have `english` as the default language and `german` as second.

Usually this gives the editorial staff a really hard time, because every bit of multilingual content has to be replaced by hand. Beside approaches that imposes redirects.


## Solution

The script iterates recursively over multilingual fields of different types and swap the content between two languages.

Create a new document, copy&paste the code below, save it to the root of your PW installation, adapt params (s. instructions within the script file) and open it in a browser.

```php
<?php namespace ProcessWire;

/**
 * Swap the content of multilingual fields between two languages
 * 
 * - Tested with ProcessWire 3.0.x, PHP 8.4.x
 * 
 * Usage:
 * 
 * Place the script file in the root of your PW installation (or
 * adapt the pw bootstrap path), adapt params (s. below), than open
 * the file in the browser.
 * 
 * @author Olaf Gleba
 * @version 1.0.0
 */

/**
 * USAGE, VERBOSE
 * 
 * 1. Place the (uncommented) `Code example` at the end of this script file 
 * 2. Adapt `<your-language-name>` with the name of the language that should
 * become default.
 * 2. Adapt `<your-template-name>` with the required template names
 * (assuming that multiple templates are to consider). Execute the script
 * file (e.g. reload the browser) ``each time`` you adapt the template name.
 * 
 * Code example:
 * 
 * $_pages = wire('pages')->find("template=<your-template-name>");
 * swapMultiLanguageContent($_pages, 'default', '<your-language-name>');
 * 
 */

/**
 * MANDATORY STEPS AFTER FINAL EXECUTION
 * 
 * After you run all considered templates you need to do:
 *
 * 1. Assuming `german` was your default language so far (name `default`)
 * and you want to define a second language, for example `english`
 * (name `english`), as the new default, adapt the name (and surely the
 * title too) on both related languages (e.g. english => default,
 * german => german).
 * 
 * 2. Go to your Homepage ("/") and in the settings tab, adapt the url of your
 * new default language and the url of your swapped language (e.g. english
 * => /, german => /de/).
 */


// bootstrap processwire
require_once './index.php';


/**
 * Function to swap the content of multilingual fields between two languages
 * 
 * 1. Page name swap
 * 2. Execute recursive function `swapFieldValuesRecursive`
 * 3. Save pages array
 */

function swapMultiLanguageContent($pages, $langA = 'default', $langB = 'deutsch') {

    $languages = wire('languages');
    $lA = $languages->get($langA);
    $lB = $languages->get($langB);

    if(!$lA || !$lB) throw new WireException("Language not found");

    foreach($pages as $page) {

        if(!$page instanceof Page || !$page->id) continue;

        $page->of(false);

        /* 1 */
        try {
            $nameA = $page->localName($lA);
            $nameB = $page->localName($lB);

            if($nameA !== $nameB) {

                $page->set("status$langB", 1);

                $page->set("name$langA", $nameB);
                $page->set("name$langB", $nameA);
            }
        } catch(Exception $e) {
            wire('log')->save('swap-lang', "Page {$page->id}: Name swap error: " . $e->getMessage());
        }

        /* 2 */
        swapFieldValuesRecursive($page, $lA, $lB);

        /* 3 */
        try {
            $page->save();
        } catch(Exception $e) {
            wire('log')->save('swap-lang', "Page {$page->id}: Save error: " . $e->getMessage());
        }

        $page->of(true);
    }
    
    return "Done.";
}


/**
 * Internal function to swap field values recursively
 * 
 * NOT intended to use as stand-alone, s. func `swapMultiLanguageContent`
 * 
 * 1. Ordinary field type
 * 2. Repeater field type
 * 3. RepeaterMatrix field type
 */

function swapFieldValuesRecursive(Page $page, Language $lA, Language $lB) {

    foreach($page->template->fieldgroup as $field) {

        $fieldname = $field->name;
        if(!$page->hasField($fieldname)) continue;

        $value = $page->$fieldname;

        if($value === null) continue;

        /* 1 */
        $type = $field->type;

        if($type instanceof FieldtypeLanguageInterface) {

            try {
                $valueA = $value->getLanguageValue($lA);
                $valueB = $value->getLanguageValue($lB);

                if($valueA != $valueB) {
                    $value->setLanguageValue($lA, $valueB);
                    $value->setLanguageValue($lB, $valueA);
                }

            } catch(Exception $e) {
                wire('log')->save('swap-lang', "Field {$fieldname} error: " . $e->getMessage());
            }

            continue;
        }

        /* 2 */
        if($type instanceof FieldtypeRepeater && $value->count()) {

            foreach($value as $repItem) {
                $repItem->of(false);
                swapFieldValuesRecursive($repItem, $lA, $lB);
                $repItem->save();
                $repItem->of(true);
            }

            continue;
        }

        /* 3 */
        if($type instanceof FieldtypeRepeaterMatrix && $value->count()) {

            foreach($value as $matrixItem) {
                $matrixItem->of(false);
                swapFieldValuesRecursive($matrixItem, $lA, $lB);
                $matrixItem->save();
                $matrixItem->of(true);
            }

            continue;
        }

        // Ignore all other field types...
    }
}
```