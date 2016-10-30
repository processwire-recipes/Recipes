title: Create an index / linklist of all used image tags

----

version: 1.0.0

----

authors: soma, teppo

----

tags: images, index

----

problem:
You need to collect all tags from image fields and create a link list to then filter pages with images that have the selected tag.

----

solution:
The solution in it's entirety can be seen below. Code comments describe exactly what is happening, and for the most part this should be pretty easy to grasp with some basic understanding of PHP and the ProcessWire API.
```PHP
/**
 * collect all tags
 * ======================================
 */

$alltags = array(); // container
$use_urlsegments = false;

// find all pages that have images with tags
$parray = $pages->find("template=basic-page|gallery, images.tags!=''");

// loop pages found and collect tags from images
foreach($parray as $p) {

    // find all images that have no empty tags, yeah you can
    // also use find on Pagefiles array!
    $images = $p->images->find("tags!=''");

    // loop them and add tags to array
    foreach($images as $im) {
        $tags = $im->tags;

        // convert "," and "|" to space and create array using explode
        if(strpos($tags, ',') !== false) $tags = str_replace(',', ' ', $tags);
        if(strpos($tags, '|') !== false) $tags = str_replace('|', ' ', $tags);
        $tags = explode(' ', $tags);

        // convert tag value to a page name using beautifyer, ü => ue, ö => oe
        // since special chars are not allowed in PW urls
        foreach($tags as $tag) {
            $alltags[$sanitizer->pageName($tag, Sanitizer::translate)] = $tag;
        }
    }
}

/**
 * generate links with tags as urlsegment
 * ======================================
 */

// make the array unique and create a tags nav from it
// add tags to the url of the page to later read it and
// render a list of pages

echo "<ul>";
foreach(array_unique($alltags) as $key => $tag) {
    if($use_urlsegments) {
        echo "<li><a href='{$page->url}$key'>$tag</a></li>";
    } else {
        echo "<li><a href='{$page->url}?tag=$tag'>$tag</a></li>";
    }
}
echo "</ul>";

/**
 * find all pages with the supplied tag
 * ======================================
 */

// enable url segments on the template if using url segments
if($input->urlSegment1 || $input->get->tag){

    if($input->urlSegment1) {
        $tagvalue = $input->urlSegment1;
        // get the original tag value text from the cached array
        $tagvalue = $alltags[$tagvalue];
    }
    if($input->get->tag) {
        $tagvalue = $sanitizer->selectorValue($input->get->tag);
    }

    // find pages with images having the requested tag
    $pa = $pages->find("images.tags~='$tagvalue'");
    if(count($pa)) {
        echo "<h2>Pages found</h2>";
        echo "<ul>";
        foreach($pa as $p) echo "<li><a href='$p->url'>$p->title</a></li>";
        echo "</ul>";
    }
}
```

----

resources:
* [ProcessWire Weekly 124](https://weekly.pw/issue/124/)
* [Soma'original recipe](https://gist.github.com/somatonic/5808897)
