# CSS Styleguide

All these years of experience and testing join in one file. Yes, this is my css styleguide. Enjoy.

## Coding Style

* Use soft-tabs with a four space indent.
* Put spaces after : in property declarations.
* Put spaces before { in rule declarations.
* Use hex color codes #000000 unless using rgba.
* Use // for comment blocks (instead of /* */).

Here is good example syntax:

``` css
// This is a good example!
.styleguide-format {
    border: 1px solid #0fff00;
    color: #000000;
    background: rgba(0,0,0,0.5);
}
```

## SCSS Style

Any ```$variable``` or ```@mixin``` that is used in more than one file should be put in ```globals/```. Others should be put at the top of the file where they're used.
As a rule of thumb, don't nest further than 3 levels deep. If you find yourself going further, think about reorganizing your rules (either the specificity needed, or the layout of the nesting).

## File Organization

In general, the CSS file organization should follow something like this:

    styles
    ├── components
    │   ├── comments.scss
    │   └── listings.scss
    ├── globals
    │   ├── browser_helpers.scss
    │   ├── responsive_helpers.scss
    │   ├── variables.scss
    ├── plugins
    │   ├── jquery.fancybox.css
    │   └── reset.scss
    ├── sections
    │   ├── issues.scss
    │   ├── profile.scss
    └── shared
        ├── forms.scss
        └── markdown.scss

## Pixels vs. Ems

Use ```px``` for ```font-size```, because it offers absolute control over text. Additionally, unit-less ```line-height``` is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the ```font-size```.

## Class naming conventions

Never reference ```js-``` prefixed class names from CSS files. ```js-``` are used exclusively from JS files.

Use the ```is-``` prefix for state rules that are shared between CSS and JS.

## Specificity (classes vs. ids)

Elements that occur exactly once inside a page should use IDs, otherwise, use classes. When in doubt, use a class name.

* Good candidates for ids: header, footer, modal popups.
* Bad candidates for ids: navigation, item listings, item view pages (ex: issue view).

When styling a component, start with an element + class namespace (prefer class names over ids), prefer direct descendant selectors by default, and use as little specificity as possible. Here is a good example:

``` html
<ul class="category-list">
  <li class="item">Category 1</li>
  <li class="item">Category 2</li>
  <li class="item">Category 3</li>
</ul>
```

``` css
ul.category-list { // element + class namespace

  &>li { // direct descendant selector > for list items
    list-style-type: disc;
  }

  a { // minimal specificity for all links
    color: #ff0000;
  }
}
```

## CSS Specificity guidelines

* If you must use an id selector (#selector) make sure that you have no more than one in your rule declaration. A rule like #header .search #quicksearch { ... } is considered harmful.
* When modifying an existing element for a specific use, try to use specific class names. Instead of .listings-layout.bigger use rules like .listings-layout.listings-bigger. Think about ack/greping your code in the future.
* The class names disabled, mousedown, danger, hover, selected, and active should always be namespaced by a class (button.selected is a good example).