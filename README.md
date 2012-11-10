[![Build Status](https://secure.travis-ci.org/markdalgleish/stellar.js.png)](http://travis-ci.org/markdalgleish/stellar.js)

# [Stellar.js](http://markdalgleish.com/projects/stellar.js/) [![endorse](http://api.coderwall.com/markdalgleish/endorsecount.png)](http://coderwall.com/markdalgleish)

### Parallax scrolling made easy

Full guide and demonstrations available at the [official Stellar.js project page](http://markdalgleish.com/projects/stellar.js/).

## Installation

Download the [development](https://raw.github.com/markdalgleish/stellar.js/master/jquery.stellar.js) or [production](https://raw.github.com/markdalgleish/stellar.js/master/jquery.stellar.min.js) version, or use a [package manager](https://github.com/markdalgleish/stellar.js#package-managers).

## Getting Started

Stellar.js is a jQuery plugin that provides parallax scrolling effects to any scrolling element. The first step is to run `.stellar()` against the element:

``` js
// For example:
$(window).stellar();
// or:
$('#main').stellar();
```

If you're running Stellar.js on 'window', you can use the shorthand:

``` js
$.stellar();
```

This will look for any parallax backgrounds or elements within the specified element and reposition them when the element scrolls.

## Parallax Elements

If you want elements to scroll at a different speed, add the following attribute to any element with a CSS position of absolute, relative or fixed:

``` html
<div data-stellar-ratio="2">
```

The ratio is relative to the natural scroll speed, so a ratio of 0.5 would cause the element to scroll at half-speed, a ratio of 1 would have no effect, and a ratio of 2 would cause the element to scroll at twice the speed.

If a ratio lower than 1 is causing the element to appear jittery, try setting its CSS position to fixed.

## Parallax Backgrounds

If you want an element's background image to reposition on scroll, simply add the following attribute:

``` html
<div data-stellar-background-ratio="0.5">
```

As with parallax elements, the ratio is relative to the natural scroll speed. For ratios lower than 1, to avoid jittery scroll performance, set the element's CSS 'background-attachment' to fixed.

## Configuring Offsets

Stellar.js' most powerful feature is the way it aligns elements.

All elements will return to their original positioning when their offset parent meets the edge of the screen—plus or minus your own optional offset. This allows you to create intricate parallax patterns very easily.

Confused? [See how offsets are used on the Stellar.js home page.](http://markdalgleish.com/projects/stellar.js/#show-offsets)

To modify the offsets for all elements at once, pass in the options:

``` js
$.stellar({
  horizontalOffset: 40,
  verticalOffset: 150
});
```

You can also modify the offsets on a per-element basis using the following data attributes:

``` html
<div data-stellar-ratio="2"
     data-stellar-horizontal-offset="40"
     data-stellar-vertical-offset="150">
```

## Configuring Offset Parents

By default, offsets are relative to the element's offset parent. This mirrors the way an absolutely positioned element behaves when nested inside an element with a relative position.

As with regular CSS, the closest parent element with a position of relative or absolute is the offset parent.

To override this and force the offset parent to be another element higher up the DOM, use the following data attribute:

``` html
<div data-stellar-offset-parent="true">
```

The offset parent can also have its own offsets:

``` html
<div data-stellar-offset-parent="true"
     data-stellar-horizontal-offset="40"
     data-stellar-vertical-offset="150">
```

Similar to CSS, the rules take precedence from element, to offset parent, to JavaScript options.

Confused? [See how offset parents are used on the Stellar.js home page.](http://markdalgleish.com/projects/stellar.js/#show-offset-parents)

Still confused? [See what it looks like with its default offset parents.](http://markdalgleish.com/projects/stellar.js/#show-offset-parents-default) Notice how the alignment happens on a per-letter basis? That's because each letter's containing div is its default offset parent.

By specifying the h2 element as the offset parent, we can ensure that the alignment of all the stars in a heading is based on the h2 and not the div further down the DOM tree.

## Configuring Scroll Positioning

You can define what it means for an element to 'scroll'. Whether it's the element's scroll position that's changing, its margins or its CSS3 'transform' position, you can define it using the 'scrollProperty' option:

``` js
$('#gallery').stellar({
  scrollProperty: 'transform'
});
```

This option is what allows you to run [Stellar.js on iOS](http://markdalgleish.com/projects/stellar.js/demos/ios.html).

You can even define how the elements are repositioned, whether it's through standard top and left properties or using CSS3 transforms:

``` js
$('#gallery').stellar({
  positionProperty: 'transform'
});
```

Don't have the level of control you need? Write a plugin!

Otherwise, you're ready to get started!

## Configuring Everything

Below you will find a complete list of options and matching default values:

``` js
$.stellar({
  // Set scrolling to be in either one or both directions
  horizontalScrolling: true,
  verticalScrolling: true,

  // Set the global alignment offsets
  horizontalOffset: 0,
  verticalOffset: 0,

  // Select which property is used to calculate scroll.
  // Choose 'scroll', 'position', 'margin' or 'transform',
  // or write your own 'scrollProperty' plugin.
  scrollProperty: 'scroll',

  // Select which property is used to position elements.
  // Choose between 'position' or 'transform',
  // or write your own 'positionProperty' plugin.
  positionProperty: 'position',

  // Enable or disable the two types of parallax
  parallaxBackgrounds: true,
  parallaxElements: true,

  // Hide parallax elements that move outside the viewport
  hideDistantElements: true,

  // Set how often the viewport size is detected
  viewportDetectionInterval: 1000,

  // Customise how elements are shown and hidden
  hideElement: function($elem) { $elem.hide(); },
  showElement: function($elem) { $elem.show(); }
});
```

## Writing a Scroll Property Plugin

Out of the box, Stellar.js supports the following scroll properties:
'scroll', 'position', 'margin' and 'transform'.

If your method for creating a scrolling interface isn't covered by one of these, you can write your own. For example, if 'margin' didn't exist yet you could write it like so:

``` js
$.stellar.scrollProperty.margin = {
  getTop: function($element) {
    return parseInt($element.css('margin-top'), 10) * -1;
  },
  setTop: function($element, val) {
    $element.css('margin-top', val);
  },

  getLeft: function($element) {
    return parseInt($element.css('margin-left'), 10) * -1;
  },
  setLeft: function($element, val) {
    $element.css('margin-left', val);
  }
}
```

Now, you can specify this scroll property in Stellar.js' configuration.

``` js
$.stellar({
  scrollProperty: 'margin'
});
```

## Writing a Position Property Plugin

Stellar.js has two methods for positioning elements built in: 'position' for modifying its top and left properties, and 'transform' for using CSS3 transforms.

If you need more control over how elements are positioned, you can write your own setter functions. For example, if 'position' didn't exist yet, it could be written as a plugin like this:

``` js
$.stellar.positionProperty.position = {
  setTop: function($element, newTop, originalTop) {
    $element.css('top', newTop);
  },
  setLeft: function($element, newLeft, originalLeft) {
    $element.css('left', newLeft);
  }
}
```

Now, you can specify this position property in Stellar.js' configuration.

``` js
$.stellar({
  positionProperty: 'position'
});
```

## Package Managers

Stellar.js can be installed with the following tools.

### [Yeoman](http://yeoman.io/)

``` bash
$ yeoman install jquery.stellar
```

### [Bower](http://twitter.github.com/bower/)

``` bash
$ bower install jquery.stellar
```

## Sites Using Stellar.js

* [Magic City](http://mc.starz.com)
* [National Geographic - Alien Deep Interactive](http://channel.nationalgeographic.com/channel/alien-deep/interactives/alien-deep-interactive)
* [François Hollande](http://www.parti-socialiste.fr/latimelineduchangement)
* [IT Support London](http://www.itsupportlondon.com)
* [Ashford University](http://bright.ashford.edu)
* [WS Interactive](http://www.ws-interactive.fr/methode)
* [Carnival of Courage](http://www.carnivalofcourage.com.au)
* [Ian Poulter](http://www.ianpoulter.com)
* [360 Strategy Group](http://360strategygroup.com)

I'm sure there are heaps more. [Let me know if you'd like me to feature your site here.](http://twitter.com/markdalgleish)

## How to Build

Stellar.js uses [Grunt](http://gruntjs.com) and [PhantomJS](http://phantomjs.org/).

Once you've got Grunt and PhantomJS set up, you can validate, test, concatenate and minify the project with the following command:

`grunt`

Each of the build steps are also available individually.

To test the code using QUnit and PhantomJS:

`grunt qunit`

To validate the code using JSHint:

`grunt lint`

To continuously validate and test the code while developing:

`grunt watch`

## Contributing to Stellar.js

Make sure that all plugin changes are made in `src/jquery.stellar.js` (`/jquery.stellar.js` and `/jquery.stellar.min.js` are generated by Grunt), and that you successfully test and build the project with `grunt` before committing.

If you want to contribute in a way that changes the API, please file an issue before submitting a pull request so we can discuss how to appropriately integrate your ideas.

## Questions?

Contact me on GitHub or Twitter: [@markdalgleish](http://twitter.com/markdalgleish)

## License

Copyright 2012, Mark Dalgleish  
This content is released under the MIT license  
http://markdalgleish.mit-license.org