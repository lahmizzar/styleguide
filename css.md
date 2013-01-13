0. Overview
1. Buttons
2. Forms
3. Source Code
4. Text Styling
5. Listings
6. Boxed Groups
7. Icons
8. Navigation
9. Behavior
10. Discussion
11. Colors
12. Animations
13. Select Menu

CSS Styleguide

Welcome to the GitHub CSS Styleguide. It's pretty rad. Before reading this, you should have a general understanding for specificity, the SCSS syntax, and KSS documentation..

While we port our styles over to SCSS with KSS documentation, please make sure to upgrade an entire element's CSS at once. Do not mix small amounts of SCSS in with plain CSS. Do your future self a favor.

If you're visiting from the internet, feel free to learn from our style. This is a guide we use for our own apps internally at GitHub. We encourage you to set up one that works for your own team.
Coding Style

    Use soft-tabs with a two space indent.
    Put spaces after : in property declarations.
    Put spaces before { in rule declarations.
    Use hex color codes #000 unless using rgba.
    Use // for comment blocks (instead of /* */).
    Document styles with KSS.

Here is good example syntax:

    // This is a good example!
    .styleguide-format {
      border: 1px solid #0f0;
      color: #000;
      background: rgba(0,0,0,0.5);
    }

SCSS Style

    Any $variable or @mixin that is used in more than one file should be put in globals/. Others should be put at the top of the file where they're used.
    As a rule of thumb, don't nest further than 3 levels deep. If you find yourself going further, think about reorganizing your rules (either the specificity needed, or the layout of the nesting).

File Organization

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
    │   ├── jquery.fancybox-1.3.4.css
    │   └── reset.scss
    ├── sections
    │   ├── issues.scss
    │   ├── profile.scss
    └── shared
        ├── forms.scss
        └── markdown.scss

Use Sprockets to require files. However, you should explicitly import any scss that does not generate styles (globals/) in the particular SCSS file you'll be needing it's helpers in. Here's a good example:

//= require_tree ./plugins
//= require my_awesome_styles

@import "../globals/basic";

.rule { ... }

Pixels vs. Ems

Use px for font-size, because it offers absolute control over text. Additionally, unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.
Class naming conventions

Never reference js- prefixed class names from CSS files. js- are used exclusively from JS files.

Use the is- prefix for state rules that are shared between CSS and JS.
Specificity (classes vs. ids)

Elements that occur exactly once inside a page should use IDs, otherwise, use classes. When in doubt, use a class name.

    Good candidates for ids: header, footer, modal popups.
    Bad candidates for ids: navigation, item listings, item view pages (ex: issue view).

When styling a component, start with an element + class namespace (prefer class names over ids), prefer direct descendant selectors by default, and use as little specificity as possible. Here is a good example:

<ul class="category-list">
  <li class="item">Category 1</li>
  <li class="item">Category 2</li>
  <li class="item">Category 3</li>
</ul>

ul.category-list { // element + class namespace

  &>li { // direct descendant selector > for list items
    list-style-type: disc;
  }

  a { // minimal specificity for all links
    color: #ff0000;
  }
}

CSS Specificity guidelines

    If you must use an id selector (#selector) make sure that you have no more than one in your rule declaration. A rule like #header .search #quicksearch { ... } is considered harmful.
    When modifying an existing element for a specific use, try to use specific class names. Instead of .listings-layout.bigger use rules like .listings-layout.listings-bigger. Think about ack/greping your code in the future.
    The class names disabled, mousedown, danger, hover, selected, and active should always be namespaced by a class (button.selected is a good example).

Experimental features (staff only)

We like to ship staff only and experimental features. This requires some discipline when writing CSS since the existing feature and experimental feature's CSS will be served simultaneously. Always keep these goals in mind:

    Style new features without affecting existing features styling.
    Easily remove experimental feature CSS if they don't work out.
    Easily remove CSS for old features when new features ship.

When developing beta or experimental features, replace the root namespace with a -experimental variant and deprecate the existing root node. In general it's better to duplicate styles in experimental blocks than try and extend the existing styles.

Given an existing feature:

<div class="notifications">
  <ul class="navigation">
    <li><a href="#">Notifications</a></li>
    <li><a href="#">Messages</a></li>
  </ul>
  <div class="notifications-listing">
    <a href="#">dragon commented on Issue #551</a>
    <a href="#">mojombo commented on Issue #91</a>
    <a href="#">defunkt uploaded a new file to defunkt/resque</a>
  </div>
</div>

// Deprecated: Existing notifications + messages design. To be removed when
// notifications-next ships.
//
// Styleguide 4.5.1
.notifications {
  ul.navigation {
    float: left;
    width: 200px;
    background: #eee;
  }

  .notification-listing {
    &>a {
      display: block;
      font-weight: bold;
    }
  }
}

A good experimental version would be:

<div class="notifications-experimental">
  <h2>New hot notifications!</h2>
  <ul class="navigation">
    <li><a href="#">Important</a></li>
    <li><a href="#">@mentions</a></li>
    <li><a href="#">Everything</a></li>
  </ul>
  <ul class="notifications-listing">
    <a href="#">dragon commented on Issue #551</a>
    <a href="#">mojombo commented on Issue #91</a>
    <a href="#">defunkt uploaded a new file to defunkt/resque</a>
  </ul>
</div>

// Experimental: Notifications, the new hotness version.
//
// Styleguide 4.5.2
.notifications-experimental {
  ul.navigation {
    float: right;
    width: 250px;
  }

  .notification-listing {
    &>a {
      float: right;
      color: #ff0000;

    }
  }
}

And of course, remember to rename your rules when experimental features ship.
