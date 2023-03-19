---
title: "Inline critical above the fold css"
version: 1.1.1
authors:
  - felixwahner
  - neuwaerts
tags:
  - css
  - performance
  - fold
date: 2015-03-15
---

## Problem

You've already optimized your website for Google Pagespeed but it keeps complaining you should "[inline critical / above the fold CSS](https://developers.google.com/speed/pagespeed/service/PrioritizeCriticalCss)".

## Solution

_This solution depends on several tools that make the "build" process a lot easier. If you've got any questions about how to use them don't hesitate to ask in the related forum thread which can be found in the resources._

## Tools used:

- [NodeJS](https://nodejs.org/)
- [Grunt](https://gruntjs.com/)
- [Grunt Critical-CSS](https://github.com/filamentgroup/grunt-criticalcss)
- [Grunt-Contrib-Copy](https://github.com/gruntjs/grunt-contrib-copy)

## Directory structure

We use the following (simplified) directory structure for all of our projects:

```
+-- /htdocs
|   +-- index.php
|   +-- /wire
|   +-- /site
|       +-- /modules
|       +-- /templates
|           +-- /assets
|               +-- /styles
|                   +-- main.css
| 					+-- critical.css
|   +-- [...]
+-- /src
|   +-- /styles
|       +--- main.(s)css
|   +-- /scripts
|   +-- [...]
+-- Gruntfile.js
```

Our directory setup is as follows: The main (processwire) application is located under "/htdocs". On the same level there is a directory called "/src" which contains all scss, uncompressed images, bower components, composer packages, js modules and so on. The reason is quite simple: We want to avoid deploying too much useless stuff (js modules, scss files, readmes from vendor scripts...) to production servers as well as keeping the "core" application as slick as possible. Of course you can also ditch all of this and just use your "good ol' handwritten css, arrrrr!" if you're not that much into using that fancy preprocessors and package managers.

## Installing the Toolchain

[Skip to the interesting part if you're already familiar with how to install and use NodeJS and GruntJS](#setup)

If you don't have NodeJS and GruntJS already installed, it's about time you install them now. Here are two tutorials that help you getting started: [Mac](http://gruntjs.com/installing-grunt) | [Windows](http://www.codebelt.com/javascript/install-grunt-js-on-windows/).

If you've finished installing open up a command prompt, `cd` to your projects folder. Use the following command to install the required grunt modules:

```bash
npm install grunt-criticalcss --save-dev
```

Don't be afraid: There will be quite a bunch of stuff downloading and producing output. Just let the package manager do it's work and go get yourself a coffee (or a beer if you prefer) in the mean time.

_Troubleshooting tip: If there is a message that says "missing package.json" just do a `npm init` and follow the on screen instructions. Retry the `npm install [...]` part afterwards._

If everything went well you should be back in your console at some point. Now it's time for:

<a name="setup"></a>

## Setting up the Grunt file

Copy and paste the following Code to the `Gruntfile.js` inside your projects directory (at the root level). Don't forget to adjust the path settings to your needs as well as changing the url to the one of your project. If your website is smaller or bigger than 1200px you can of course adjust those settings, too.

```js
module.exports = function (grunt) {
  grunt.initConfig({
    paths: {
      src: "src/",
      srcAssets: "<%= paths.src %>assets/",
      srcStyles: "<%= paths.srcAssets %>styles/",
      tmpl: "htdocs/site/templates/",
      assets: "<%= paths.tmpl %>assets/",
      styles: "<%= paths.assets %>styles/",
    },

    criticalcss: {
      custom: {
        options: {
          url: "http://localhost/mycoolproject/htdocs/",
          ignoreConsole: true,
          forceInclude: [".classes-that-need-to-be-included"],
          width: 1200,
          height: 900,
          outputfile: "<%= paths.styles %>critical.css",
          filename: "<%= paths.styles %>main.css",
          buffer: 900 * 1200,
        },
      },
    },

    copy: {
      assets: {
        expand: true,
        cwd: "<%= paths.srcStyles %>",
        src: "*.{css}",
        dest: "<%= paths.styles %>",
      },
    },
  });

  grunt.loadNpmTasks("grunt-contrib-sass");
  grunt.registerTask("default", ["criticalcss", "copy:assets"]);
};
```

This configuration assumes there is a `main.css` located in `src/assets/styles/` and your website runs under `http://localhost/mycoolproject/htdocs/`. The URL is crucial as there will be some "headless browser magic" done if you start the critical-css task. To explain this further here is what happens if you start the Grunt task with the command you'll find later on:

- A webkit based browser that runs without a graphical user interface ([PhantomJS](http://phantomjs.org/)) is started
- The browser renders the given url and checks which styles are displayed within the configuration dimensions rectangle (1200x900px in this case)
- A list of styling rules to be applied is beeing generated
- The rules are extracted from your `main.css` and inserted into a (newly created) `critical.css`
- Both files are copied into your `htdocs/site/templates/assets/styles` folder (you can also remove this when you don't need it and do all of this stuff within your "normal" assets folder).

## Preparing the template

_Although we're using [TemplateTwigReplace](http://modules.processwire.com/modules/template-twig-replace/) and [TemplateDataProviders](http://modules.processwire.com/modules/template-data-providers/) for our projects I assume you don't (you should, though! :)). So for the sake of simplicity we'll be using "Plain old PHP" in this example._

The next step is to open up your include file that contains your pages metadata and assets. I'm assuming here that in most cases this will be something similar to `site/templates/includes/head.inc.php`.

Next find the place where you include your main CSS file. In my case it's named 'main.css' (as seen in the Gruntfile.js).

Now replace your `<link [...]>` with the following code (taken from [https://adactio.com/journal/8504](https://adactio.com/journal/8504) and "converted" to php):

```php
<?php
	$cssupdate 			= '20150330';
	$cssCookie 			= wire('input')->cookie->csscached;
	$cssPath 			= $config->paths->templates . 'assets/styles/';
	$cssFileMain 		= $cssPath . 'main.css';
	$cssFileCritical 	= $cssPath . 'critical.css';
?>
<?php if(isset($cssCookie) && $cssCookie == $cssupdate) : ?>
	<link rel="stylesheet" href="<?php echo $cssFileMain; ?>" /> <?php else : ?>
	<style>
		<?php include $cssFileCritical; ?>
	</style>
	<noscript>
	    <link rel="stylesheet" href="<?php echo $cssFileMain; ?>" />
	</noscript>
	<script>
	(function (win, doc) {
	    'use strict';
		function loadCSS(e){function t(){var e,n;for(n=0;n<l.length;n+=1)l[n].href&&l[n].href.indexOf(r.href)>-1&&(e=!0);e?r.media="all":win.setTimeout(t)}var r=doc.createElement("link"),n=doc.getElementsByTagName("script")[0],l=doc.styleSheets;return r.rel="stylesheet",r.href=e,r.media="only x",n.parentNode.insertBefore(r,n),t(),r}
	    loadCSS('<?php echo $cssFileMain; ?>');
	    doc.cookie = 'csscached=<?php echo $cssupdate; ?>;expires="Tue, 19 Jan 2038 03:14:07 GMT";path=/';
	}(this, this.document));
	</script>
<?php endif; ?>
```

What happens here is that we check if a cookie named "cssupdate" with a value of "20150330" exists.
If this is the case the user has already visited our website and the chances are good the "main.css"
is already cached in his or her browser.
If this isn't the case the content of our "critical.css" file is included inline.
Also there is a fallback (just load the main.css instantly) for the users that browse with javascript disabled inside the `<noscript></noscript>` tags.
Last but not least we're loading the main.css file asynchronously using
[loadCSS](http://martinwolf.org/2014/12/18/load-css-asynchronously-with-loadcss/) and set a cookie named "csscached" with the date string we previously defined in `$cssupdate` and a far future expires date to make it last in the cache for a long time.

## Ready, set, ...

Finally type `grunt` and Press `<Enter>` (it feels right to press a little harder than normal) in your command line, lean back and watch the magic happen.

![Go](http://i.imgur.com/fBiNqXW.gif)

## Further optimization and optimizing

At this point you have to manually edit the `$cssupdate` string everytime you're updating your css. This could also be automated using a grunt plugin like [grunt-cache-bust](https://github.com/hollandben/grunt-cache-bust) in combination with the .htaccess rules that are explained in the [original post](https://adactio.com/journal/8504).

---

### Resources

- [Original post by adactio](https://adactio.com/journal/8504)
- [Forum thread](https://processwire.com/talk/topic/4710-frontend-performance-tips/?p=90612)
- [PageSpeed: Inline critical / above the fold CSS](https://developers.google.com/speed/pagespeed/service/PrioritizeCriticalCss)
