# CSS & Utility Classes

* * *

## Summary

This document covers the approach to CSS and Utility Classes that is adopted for Be Devious Web Development Websites.

As a general rule, we try to write as little CSS as possible and instead opt for Utility Classes where possible, which could be from one of the following sources:

* The version of Bootstrap used on the site (currently new sites use [Bootstrap 4](https://getbootstrap.com/docs/4.6/getting-started/introduction/))
* The [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) Plugin
* Utilities local to the site you are working on

## Where to Write CSS

As far as possible, CSS should be added using available interfaces in the WordPress admin area rather than directly editing any theme or plugin files. Therefore CSS should be added to a site in the following locations:

* In the CSS section of a Toolset Content Template where the CSS _only_ refers to the given Content Template
* In the WordPress Customiser under Custom CSS

## CSS Coding Standards

General coding standards [have moved here](/wiki/spaces/BDWD/pages/1159888897/CSS+Standards).

## Required on every project: remove the default focus / active ring

Bootstrap 4 (through `bb-theme`) draws an outline and a blue box-shadow glow on `:focus`,
`:focus-visible` and `:active` for links, buttons, nav items, form controls and anything
with a `tabindex`. It shows up as a stray border when a logo, nav link, toggle, slider
arrow or button is clicked or tapped.

**Every child theme must include the fix in its `style.css`.** It is not optional: every
project has needed it, and the ones that skipped it were patched later.

Use a single project-wide rule near the top of `style.css`, not per-component rules, so
new interactive elements are covered automatically:

```css
a:focus, a:focus-visible, a:active,
button:focus, button:focus-visible, button:active,
.btn:focus, .btn:focus-visible, .btn:active,
input[type="submit"]:focus, input[type="submit"]:focus-visible, input[type="submit"]:active,
.navbar-brand:focus, .navbar-brand:focus-visible, .navbar-brand:active,
.navbar-toggler:focus, .navbar-toggler:focus-visible, .navbar-toggler:active,
.nav-link:focus, .nav-link:focus-visible, .nav-link:active,
input:focus, input:focus-visible,
textarea:focus, textarea:focus-visible,
select:focus, select:focus-visible,
.form-control:focus, .form-control:focus-visible,
[role="button"]:focus, [role="button"]:focus-visible, [role="button"]:active,
[tabindex]:focus, [tabindex]:focus-visible {
  box-shadow: none !important;
  outline: none !important;
}

a, button, [role="button"] {
  -webkit-tap-highlight-color: transparent;
}
```

The `!important` is needed to beat Bootstrap's own box-shadow rules. The reference
implementation, with the full explanatory comment, is in
`sftp-themes/lysses-house-sftp/style.css`.

This also removes the browser's keyboard focus indicator. If a design needs a visible
keyboard focus state, give that element its own `:focus-visible` style (colour, underline
or background change) rather than restoring the glow.
