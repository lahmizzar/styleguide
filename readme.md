# Styleguide
_This is my styleguide, not yours_

## Overview

### Coding style

- Use soft-tabs with a four space indent.
- Put spaces after : in property declarations.
- Put spaces before { in rule declarations.
- Use hex color codes #000000 unless using rgba.

A good syntax example:

```
// This is a good example!
.styleguide-format {
  border: 1px solid #0f0ff0;
  color: #000000;
  background: rgba(0,0,0,0.5);
}
```

### Organisation

In general, the CSS file organization should follow something like this:

```
styles
├── components
│   ├── bootstrap.scss
│   └── jquery.bxslider.scss
├── globals
│   ├── default.scss
│   ├── layout.scss
│   ├── sprite.scss
├── views
│   ├── issues.scss
│   ├── profile.scss
└── shared
    ├── forms.scss
    └── buttons.scss
``
