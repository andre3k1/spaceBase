spaceBase
=========

### spaceBase is a Sass-based responsive CSS framework.

Many CSS frameworks are meant to be included and left alone. But spaceBase is a boilerplate layer that can be built upon and tailored for your needs. It combines best practices for today’s responsive web with the core components we use on every project. Consider it the launch pad for your adventures into cyberspace.

spaceBase is meant to be copied into your project for further modifications. The SCSS files in `scss/base/` are usually not modified much, but the files in `scss/ui/` are meant to be completely tailored to your project.

### Getting Started

After you download the spaceBase ZIP from the repo, unzip it and copy its contents into your own project.

spaceBase is built with Sass, so you'll need a Sass compiler such as Compass. A Compass `config.rb` is included in the spaceBase folder you can use for reference.

Installing Ruby, Sass, and Compass is outside the scope of this document.

Your editor should be setup with an EditorConfig plugin, so that certain basic code style standards are enforced automatically. See http://editorconfig.org/#download and install the plugin for your editor of choice.

You can remove `tests/` and `scss/README.md`.

### The spaceBase structure

```
fonts/
scss/
  base/
    ...
  ui/
    ...
  _manifest.scss
  _vars.scss
  application-ie.scss
  application.scss
  styleguide.scss
```

Read the [CSS Architecture Overview](scss/README.md) for details on these files.

### Credits

* [Harry Robert’s Inuit.css](https://github.com/csswizardry/inuit.css)
* [Bootstrap](http://getbootstrap.com)
* [HTML5 Boilerplate](http://html5boilerplate.com)
* [Normalize.css](http://necolas.github.io/normalize.css)
* [Eric Meyer’s CSS Reset](http://meyerweb.com/eric/tools/css/reset)

### License

spaceBase is free to use under the [MIT License](LICENSE.md).

Copyright 2015 [space150](http://www.space150.com)


# CSS Architecture Overview

## Philosophy

Mobile first. Fallback for legacy IE.

## Compiling

spaceBase uses Compass by default, but can be changed to Bourbon in `_manifest.scss`. Running `compass watch` (while in the parent directory of `scss/`) will watch for changes and compile your CSS properly.


## Directories

The `scss/` directory contains two folders: Base and UI.

- **Base** [avoid editing] contains things like normalize/CSS reset, mixins, helpers, and the "building block" abstractions. Most of the layout and basic visual styles can be achieved with these classes. The grid, widths, lists, blocks, and basic spacing content objects are in here.
- **UI** [customize this] contains all design-specific styles shared across the application. This folder is free to organize however the project demands. To start, it includes layout, fonts, typography, buttons, and generic print styles.


## Legacy IE Support

All partials are compiled into two CSS files in the main `stylesheets/` directory, `application.css` and `application-ie.css`. These should be loaded into the site via conditional comments.

Example:
~~~html
<!--[if (gt IE 8) | (IEMobile)]><!-->
  <link href="/stylesheets/application.css" media="all" rel="stylesheet" type="text/css" />
<!--<![endif]-->

<!--[if (lt IE 9) & (!IEMobile)]>
  <link href="/stylesheets/application-ie.css" media="all" rel="stylesheet" type="text/css" />
<![endif]-->
~~~

The only difference between these files is the `$legacy-ie` variable. When set to true, the `media-query()` mixin will only render the content inside the media query breakpoints. The site will thereby display the desktop layout in legacy IE browsers.


## Variables

Global variables are kept in the `_vars.scss` partial. This includes things like base font styles, colors, breakpoints, and some misc. sizing measurements. All of these variables are used throughout most of the partials. There are a few spots where having some "local" variables are helpful for math and reuse in that particular section. These variables have been prefixed with an underscore so you know that those variables aren't used outside that partial.


## The Grid

The grid structure comes almost directly from [inuit.css](http://inuitcss.com/2012/12/building-grid-systems-with-inuit-css/) with some minor modifications. It works on proportional sizes, not "columns" and "rows".

Example:
~~~html
<div class="grid-wrapper">

    <div class="grid two-thirds">
        ...
    </div>

    <div class="grid one-third">
        ...
    </div>

</div>
~~~

### The `$responsive` Variable

If $responsive is set to false, the grid will collapse to 100% on mobile and use the same proportions for all wider breakpoints.

If $responsive is set to true, the grid proportions will change at different breakpoints with the addition of classes prefixed with the breakpoint.

Example:
~~~html
<div class="grid-wrapper">

    <div class="grid one-whole lap-one-half desk-three-quarters">
        ...
    </div>

    <div class="grid one-whole lap-one-half desk-one-quarters">
        ...
    </div>

</div>
~~~

### The `$base-sizing-unit` Variable

As a means for consistency and good vertical rhythm as many "measurements" are based off the base-spacing-unit, which is equal to the base line-height. This way if we need to adjust down the road, all proportions are preserved.
