# General Development Flow

* * *

## Summary

This document describes the general approach to building any site or any element of a site and should be used as a guide to follow unless there are specific reasons for deviating.

## Principles

Here is a list of principles that can be applied to any work that is being done:

### Content

* Content should not be hard coded and should be linked to user interfaces enabling administrator users to easily edit content at any time without developer input.
* As much content editing as possible should be via the Beaver Builder drag and drop interface, however where this isn’t possible or practical, it should be via the WordPress admin pages in logical place.

### Code

* Any code should be, above all, easy to read. [WordPress coding guidelines](https://make.wordpress.org/core/handbook/best-practices/coding-standards/) are a good go to.
* Sites should use the currently employed version of Bootstrap and Bootstrap components should be used where possible when writing custom elements.
* As little CSS and JavaScript should be written as possible, opting instead for utility classes and code provided by existing frameworks and plugins.
* Flexbox should always be the #1 choice for complex layouts.

### Settings and Layout / Style Options

* Settings and layout / style options, e.g. font size or colour, should be applied at the highest possible level that makes sense for the case, the levels could be considered:
    * Site level: applied using settings in the customiser or in Custom CSS in the customiser
    * Row level: applied using the row settings in Beaver Builder
    * Column level: applied using the column settings in Beaver Builder
    * Module level: applied either using the module settings in Beaver Builder or CSS in a custom module using Toolset Views
* The is opposed to, for example, if the standard font size for a site is 18px, this should be set in the customiser and **not** by setting the font size for every module added to the site in Beaver Builder.

### Responsive development

* Where possible build for mobile first and scale up. i.e. in CSS you should be writing `@media( min-width:...)` a lot more than you write `@media( max-width:...)`. The same principle applies to responsive utility classes.
* Use Bootstrap default breakpoints wherever possible which will help when aligning `@media` queries with responsive utility classes.

## General Process

The general process for developing any part of a site goes something like this:

* Determine if the content is simple enough to use standard Beaver Builder modules. 99% of the time, unless the content is a very basic header / text / image layout then this will not be the case.
* If a section has multiple columns, determine if the Beaver Builder rows and columns can be used, if not, these will need to form part of the custom module. For example, a section with alternating image / text sections is likely to require columns in the custom module. This is especially true if a layout is shared with a view.
* Create a new _Content Template_ or _View_ with Toolset and set up a draft of the HTML markup with relevant fields.
* Embed the new view with the BDVS Content Template module adding in content or dummy content.
* Update the HTML with utility classes, write any additional CSS or JavaScript that is required and save to the _Content Template_ or _View_.
* Test the new layout across viewport sizes updating as necessary.