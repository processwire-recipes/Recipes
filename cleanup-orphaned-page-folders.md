---

title: "Clean up orphaned page folders and their content"

---

version: 1.0.0

---

authors: Olaf Gleba

---

tags: pages, files

---

date: 2026-01-25

---



## Problem

For your new project you duplicate a old installation so you don't have to start from scratch. While building the new website your `assets/files` folder gets messy,- there are a growing number of orphaned page id folders with obsolete content you want to get rid of.

## Solution

Create a new document, copy&paste the code below, save it to the root of your PW installation and open it in a browser. Initially it performs a dry run, so your are save before actual deletion take place.

Because we are dealing with actual files, make a backup of you `files` folder and double check the output results before changing the mode.

```php
<?php namespace ProcessWire;

/**
 * Cleanup orphaned page folders and their content
 * 
 * - Only delete page folders WITH NO existing Page-ID.
 * - Implements a dry run mode
 * - Tested with ProcessWire 3.0.x, PHP 8.4.x
 * 
 * Usage:
 * 
 * Place the script file in the root of your PW installation (or
 * adapt the pw bootstrap path), than open the file in the browser.
 * 
 * @author Olaf Gleba
 * @version 1.0.0
 */

// Bootstrap ProcessWire
require_once './index.php';

// Define mode (default `dry run`), set to `false` for actual deletion
$dryRun = true;

// Print to browser
echo "<p>Orphaned page folder cleanup</p>";
echo "<p>Mode: " . ($dryRun ? "DRY RUN - NO DELETION" : "LIVE - ACTUAL DELETION") . "</p>";

/**
 * Load existing Page-Ids from database
 */
$existingPageIds = [];

$sql = $database->query("SELECT id FROM pages");
while ($row = $sql->fetch(\PDO::FETCH_ASSOC)) {
    $existingPageIds[(int)$row['id']] = true;
}

// Print to browser
echo "<p>Existing pages loaded: " . count($existingPageIds) . "</p>";

/**
 * Iterate over the `files` folder
 */
// Get files root
$filesRoot = realpath($config->paths->files);

$removedFolders = 0;

$dirs = scandir($filesRoot);

foreach ($dirs as $dir) {

    // Only nummeric folders (Page-IDs)
    if (!ctype_digit($dir)) {
        continue;
    }

    $pageId = (int)$dir;
    $pageDir = $filesRoot . DIRECTORY_SEPARATOR . $dir;

    if (!is_dir($pageDir)) {
        continue;
    }

    // Skip if page exists
    if (isset($existingPageIds[$pageId])) {
        continue;
    }

    // Print to browser
    echo "<p>Orphaned page folder: {$pageDir}</p>";

    // List folder files
    $iterator = new \RecursiveIteratorIterator(
        new \RecursiveDirectoryIterator(
            $pageDir,
            \FilesystemIterator::SKIP_DOTS
        ),
        \RecursiveIteratorIterator::CHILD_FIRST
    );

    foreach ($iterator as $item) {
        if ($item->isFile()) {
            echo "  File: " . $item->getRealPath() . "<br />";
        }
    }

    // Delete folder (if args `--delete` is present, s.above)
    if (!$dryRun) {
        deleteDirectory($pageDir);
        echo "  FOLDER DELETED<br />";
    } else {
        echo "  (DRY RUN - NO DELETION)<br />";
    }

    echo "<br/>";
    $removedFolders++;
}

/**
 * Print summary to browser
 */
echo "<p>";
echo "---------------------------------<br />";
echo "Done<br />";
echo "Orphaned page folders: {$removedFolders}<br />";
echo "---------------------------------<br />";
echo "</p>";

/**
 * Recursive deletion
 */
function deleteDirectory(string $dir): void
{
    if (!is_dir($dir)) {
        return;
    }

    $items = scandir($dir);

    foreach ($items as $item) {
        if ($item === '.' || $item === '..') {
            continue;
        }

        $path = $dir . DIRECTORY_SEPARATOR . $item;

        if (is_dir($path)) {
            deleteDirectory($path);
        } else {
            unlink($path);
        }
    }

    rmdir($dir);
}
```