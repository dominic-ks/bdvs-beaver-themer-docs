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