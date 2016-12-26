JS Sequence Diagrams v2 [![Build Status](https://travis-ci.org/bramp/js-sequence-diagrams.svg?branch=master)](https://travis-ci.org/bramp/js-sequence-diagrams)
=============================================
**Generates UML sequence diagrams from simple text**  
<https://bramp.github.io/js-sequence-diagrams/>

by [Andrew Brampton](http://bramp.net) 2012-2017


Example
-------
We turn

    Alice->Bob: Hello Bob, how are you?
    Note right of Bob: Bob thinks
    Bob-->Alice: I am good thanks!

into

![Sample generated UML diagram](https://bramp.github.io/js-sequence-diagrams/images/sample.svg)

Requirements
------------
You will need [Snap.svg](http://snapsvg.io/), [Web Font Loader](https://github.com/typekit/webfontloader) (if you wish to use custom fonts), [underscore.js](http://underscorejs.org/) (or [lodash](https://lodash.com/)), and optionally [jQuery](https://jquery.com/).


Installation
----------------------

### bower

Run `bower install bramp/js-sequence-diagrams` and include the scripts below:

```html
<script src="{{ bower directory }}/bower-webfontloader/webfont.js" />
<script src="{{ bower directory }}/snap.svg/dist/snap.svg-min.js" />
<script src="{{ bower directory }}/underscore/underscore-min.js" />
<script src="{{ bower directory }}/js-sequence-diagrams/build/sequence-diagram-min.js" />
```

also import the CSS if you plan to use the hand drawn theme:
 
```html
<link href="{{ bower directory }}/js-sequence-diagrams/build/sequence-diagram-min.css" rel="stylesheet" />
```

Not using bower? No problem. Just download the dependencies, and include them yourself.

Usage
-----

You can use the Diagram class like:

```html
<div id="diagram">Diagram will be placed here</div>
<script> 
  var d = Diagram.parse("A->B: Does something");
  var options = {theme: 'simple'};
  d.drawSVG('diagram', options);
</script>
```

or use jQuery to do all the work:
```html
<div class="diagram">A->B: Message</div>
<script>
  var options = {theme: 'hand'};
  $(".diagram").sequenceDiagram(options);
</script>
```

For full examples check out [the demo site](https://bramp.github.io/js-sequence-diagrams/).

Options
-------

```javascript
var options = {
    // Change the styling of the diagram, typically one of 'simple', 'hand'. New themes can be registered with registerTheme(...).
    theme: string,

    // CSS style to apply to the diagram's svg tag. (Only supported if using snap.svg)
    css_class: string,
};
```

Styling
-------

The following CSS classes are applied to the SVG diagram when using snap.svg:

* `sequence`: Applies to main SVG tag.
* `title`: Applied to the title of the diagram.
* `actor`: Applied to the actors.
* `signal`: Applied to the signals.
* `note`: Applied to all notes.

The diagram can then be customised, for example:

```css
.signal text {
    fill: #000000;
}
.signal text:hover {
    fill: #aaaaaa
}
.note rect, .note path {
    fill: #ffff00;
}
.title rect, .title path,
.actor rect, .actor path {
    fill: #ffffff
}
```

Raphaël Deprecation
-------------------

Version 1.x of this library used [Raphaël](http://raphaeljs.com/) for drawing the diagrams, however, Raphaël had some limitations, and since disappeared from the Internet. We've decided to move to [Snap.svg](http://snapsvg.io/), which is a pure SVG implementation, instead of  Raphaël which in addition to SVG, also supported VML (on Internet Explorer). This support of VML made it impossible to use some newer SVG capabilities. Native SVG allows us to use CSS styling, better font support, animations and more.

To aid in the transition Version 2.x will support both Raphaël and Snap.svg (preferring Snap.svg). If you include Raphaël instead of snap.svg, it will default to using Raphaël as the rendering library. For example 

```html
<script src="{{ bower directory }}/raphael/raphael-min.js"></script>
```

There are also four transitional themes, 'snapSimple', 'snapHand', 'raphaelSimple', 'raphaelHand', which force the use of either Snap.svg, or Raphaël.

The plan is to drop support for Raphaël in a future release, simplifying the library, and reducing the file size.

### Adding a Font

Raphael requires Cufon style fonts. Find the font you want in ttf or otf format, visit [Cufon's site](http://cufon.shoqolate.com/generate/) and process it into a javascript file. Then ensure the font is included via the HTML, or recompile after altering main.js. So far only the hand drawn font, Daniel Bold, has been included.


Build requirements
------------------
The build is managed by a Makefile, and uses various tools available from npm. Thus both `make` and [npm](https://github.com/npm/npm) are required, and can easily be installed on any Linux or Mac machine.

```bash
make
```

The Makefile will use npm to install all the dev dependencies, build, and test.

Testing
-------

We use [qunit](https://qunitjs.com/) for testing. It can be ran from the command line, or via a browser. The command line actually tests multiple permutations of [lodash](https://lodash.com/), [Underscore](http://underscorejs.org/), and with and without minification.

```bash
make test
...
Global summary:
┌───────┬───────┬────────────┬────────┬────────┬─────────┐
│ Files │ Tests │ Assertions │ Failed │ Passed │ Runtime │
├───────┼───────┼────────────┼────────┼────────┼─────────┤
│ 1     │ 13    │ 231        │ 0      │ 231    │ 250     │
└───────┴───────┴────────────┴────────┴────────┴─────────┘
```

or `make` and then open test/qunit.html in a browser. Finally a simple playground is available at test/test.html.

How to release
--------------
* Make sure all changes checked in
* Bump version in src/main.js and bower.json
* ``make clean``
* ``make``
* ``git add -f src/main.js bower.json build/sequence-diagram-min.js build/sequence-diagram-min.js.map``
* ``git commit -m "Released version 1.x.x"``
* ``git push origin master``
* ``git tag -a v1.x.x -m v1.x.x``
* ``git push origin v1.x.x``


TODO
----
* Other themes
* Automate the release process
* Testing that checks the generated SVG is correct
* Improve the hand drawn theme
* Dozens of other issues on https://github.com/bramp/js-sequence-diagrams/issues

Contributors
------------

via [GitHub](https://github.com/bramp/js-sequence-diagrams/graphs/contributors)

Thanks
------
This project makes use of [Jison](https://zaach.github.io/jison/), snap.svg, underscore.js, and the awersome [Daniel font](http://www.dafont.com/daniel.font) (which is free to use for any purpose).

Many thanks to [Web Sequence Diagrams](http://www.websequencediagrams.com/) which greatly inspired this project, and forms the basis for the syntax.

Related
-------

* [Web Sequence Diagrams](http://www.websequencediagrams.com/) Server side version with a commercial offering
* [flowchart.js](https://adrai.github.io/flowchart.js/) A similar project that draws flow charts in the browser


Licence (Simplified BSD License)
-------

Copyright (c) 2012-2017, Andrew Brampton
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

- Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
- Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
