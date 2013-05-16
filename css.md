# CSS Styleguide

All these years of experience and testing join in one file. Yes, this is my css styleguide. Enjoy.

## Coding Style

The chosen code format must ensure that code is: easy to read; easy to clearly comment; minimizes the chance of accidentally introducing errors; and results in useful diffs and blames.

* Write valid CSS unless you really know what you're doing.
* Avoid ```@import```.
* Put spaces after ```:``` in property declarations.
* Put spaces before ```{``` in rule declarations.
* Use lowercase and shorthand hex values, e.g., ```#aaa``` unless using rgba.
* Use ```//``` for comment blocks (instead of ```/* */```) (Only in Sass).
* Use one discrete selector per line in multi-selector rulesets.
* Include a single space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block.
* Use one level of indentation for each declaration.
* Use single or double quotes consistently. Preference is for double quotes, e.g., ```content: ""```.
* Quote attribute values in selectors, e.g., ```input[type="checkbox"]```.
* Where allowed, avoid specifying units for zero-values, e.g., ```margin: 0```.
* Include a space after each comma in comma-separated property or function values.
* Include a semi-colon at the end of the last declaration in a declaration block.
* Place the closing brace of a ruleset in the same column as the first character of the ruleset.
* Separate each ruleset by a blank line.
* Words are separated by a dash. e.g: ```#header-navigation```. Avoid camelCase.
* Use meaningfull selectors that reflect content, not presentation.
* DO NOT USE ```!important```. Except if you really have no choice (which means never).

Here is good example syntax:

``` css
// This is a good example!
.selector-1,
.selector-2,
.selector-3[type="text"] {
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    display: block;
    font-family: helvetica, arial, sans-serif;
    color: #333;
    background: #fff;
    background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
    padding: 10px;
}
```

Oh by the way: Use the least specific selector needed.

``` css
/* bad */
#header #logo {
    ...
}

/* good */
#logo {
    ...
}
```

### Declaration order

If declarations are to be consistently ordered, it should be in accordance with a single, simple principle. My preference is for structurally important properties (e.g. positioning and box-model) to be declared prior to all others.

``` css
.selector {
    /* Positioning */
    position: absolute;
    z-index: 10;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    /* Display & Box Model */
    display: inline-block;
    overflow: hidden;
    box-sizing: border-box;
    width: 100px;
    height: 100px;
    padding: 10px;
    border: 10px solid #333;
    margin: 10px;

    /* Other */
    background: #000;
    color: #fff;
    font-family: sans-serif;
    font-size: 16px;
    text-align: right;
}
```

###  Exceptions and slight deviations

Large blocks of single declarations can use a slightly different, single-line format. In this case, a space should be included after the opening brace and before the closing brace.

``` css
.selector-1 { width: 10%; }
.selector-2 { width: 20%; }
.selector-3 { width: 30%; }
```

Long, comma-separated property values - such as collections of gradients or shadows - can be arranged across multiple lines in an effort to improve readability and produce more useful diffs. There are various formats that could be used; one example is shown below.

``` css
.selector {
    background-image:
        linear-gradient(#fff, #ccc),
        linear-gradient(#f3c, #4ec);
    box-shadow:
        1px 1px 1px #000,
        2px 2px 1px 1px #ccc inset;
}
```

## SCSS Style

Different CSS preprocessors have different features, functionality, and syntax. Your conventions should be extended to accommodate the particularities of any preprocessor in use. The following guidelines are in reference to Sass.

Any ```$variable``` or ```@mixin``` that is used in more than one file should be put in ```globals/```. Others should be put at the top of the file where they're used.
As a rule of thumb, don't nest further than 3 levels deep. If you find yourself going further, think about reorganizing your rules (either the specificity needed, or the layout of the nesting).

* Limit nesting to 1 level deep. Reassess any nesting more than 2 levels deep. This prevents overly-specific CSS selectors.
* Avoid large numbers of nested rules. Break them up when readability starts to be affected. Preference to avoid nesting that spreads over more than 20 lines.
* Always place @extend statements on the first lines of a declaration block.
* Where possible, group @include statements at the top of a declaration block, after any @extend statements.
* Consider prefixing custom functions with x- or another namespace. This helps to avoid any potential to confuse your function with a native CSS function, or to clash with functions from libraries.

``` css
.selector-1 {
    @extend .other-rule;
    @include clearfix();
    @include box-sizing(border-box);
    width: x-grid-unit(1);
    // other declarations
}
```

## File Organisation

In general, the CSS file organisation should follow something like this:

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

* If you must use an id selector (```#selector```) make sure that you have no more than one in your rule declaration. A rule like ```#header .search #quicksearch { ... }``` is considered harmful.
* When modifying an existing element for a specific use, try to use specific class names. Instead of ```.listings-layout.bigger``` use rules like ```.listings-layout.listings-bigger```. Think about ack/greping your code in the future.
* The class names ```disabled```, ```mousedown```, ```danger```, ```hover```, ```selected```, and ```active``` should always be namespaced by a class (```button.selected``` is a good example).

## Pratical exemple

An example of various conventions.

``` css
/* ==========================================================================
   Grid layout
   ========================================================================== */

/**
 * Column layout with horizontal scroll.
 *
 * This creates a single row of full-height, non-wrapping columns that can
 * be browsed horizontally within their parent.
 *
 * Example HTML:
 *
 * <div class="grid">
 *     <div class="cell cell-3"></div>
 *     <div class="cell cell-3"></div>
 *     <div class="cell cell-3"></div>
 * </div>
 */

/**
 * Grid container
 * Must only contain `.cell` children.
 */

.grid {
    height: 100%;
    /* Remove inter-cell whitespace */
    font-size: 0;
    /* Prevent inline-block cells wrapping */
    white-space: nowrap;
}

/**
 * Grid cells
 * No explicit width by default. Extend with `.cell-n` classes.
 */

.cell {
    position: relative;
    display: inline-block;
    overflow: hidden;
    box-sizing: border-box;
    height: 100%;
    /* Set the inter-cell spacing */
    padding: 0 10px;
    border: 2px solid #333;
    vertical-align: top;
    /* Reset white-space */
    white-space: normal;
    /* Reset font-size */
    font-size: 16px;
}

/* Cell states */

.cell.is-animating {
    background-color: #fffdec;
}

/* Cell dimensions
   ========================================================================== */

.cell-1 { width: 10%; }
.cell-2 { width: 20%; }
.cell-3 { width: 30%; }
.cell-4 { width: 40%; }
.cell-5 { width: 50%; }

/* Cell modifiers
   ========================================================================== */

.cell--detail,
.cell--important {
    border-width: 4px;
}
```

## Class names

Some drafts:

http://blog.kaelig.fr/post/48196348743/fifty-shades-of-bem
http://www.vanseodesign.com/css/block-element-modifier/
https://gist.github.com/necolas/1309546