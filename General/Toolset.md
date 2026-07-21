# Toolset

* * *

## Summary

The Toolset plugins are a suite of plugins that add generic functionality to WordPress and can take care of what might otherwise be repetitive coding.

This guide does not intent to reproduce the official [Toolset documentation](https://toolset.com/documentation/) but instead highlights notable options and settings and their relevance to Be Devious Web Development projects.

## General Notes

When using the code editors in Toolset, it’s good to try and keep things consistent. The editor comes with some formatting rules, so we can use these to format automatically and ensure we’re all working in the same way.

To format code in the editor, select all of it’s contents using ctrl + a (cmd on Mac) and the pressing shift + tab to align the code.

This works for HTML, CSS and JavaScript code sections.

## Custom Post Types

You can create Custom Post Types with Toolset under _Toolset > Post Types_. Some general points:

* The Toolset CPT editor lets you choose the Classic Editor for the post type, this is often useful if you are adding custom fields as the UI is nicer and if you are using [Beaver Builder](/wiki/spaces/BDWD/pages/610992129/Beaver+Builder) or a [Beaver Themer](/wiki/spaces/BDWD/pages/611287049/Beaver+Themer) Template then there is no need for the Block Editor.
* As a general rule, don’t edit the labels
* Only enable capabilities in the _Sections to display when editing_ section that are specifically needed, i.e. don’t just add the editor unless there is a body of content to be added to the CPT
* If the CPT is for managing data and will not have an archive or public facing page disable these options (e.g. testimonials), if it will have a public facing page (e.g. case studies), enable them:
    * Rewrite
    * Allow permalinks to be prepended with front base
    * Pages
    * has_archive
* If the CPT will need to be sorted in an arbitrary order, be sure to enable the _hierarchical_ option as this will enable the drag and drop sorter in the admin area using the [Simple Page Ordering plugin](Tools%20&%20Plugins.md#supporting--commonly-used-plugins)
* These options should almost always be enabled unless you specifically need otherwise:
    * show_in_menu
    * show_ui
    * publicly_queryable
* These options should almost always be disabled unless you specifically need otherwise:
    * exclude_from_search
    * can_export
    * show_in_nav_menus
    * query_var
    * show_in_rest

## Custom Post Type Relationships

- [ ] Add some info on relationships

## Custom Fields

You can create custom fields for posts, terms and users under _Toolset > Custom Fields_. It is rare to use the terms and user fields, so mostly it will be for posts. Some general points:

* Where possible assign one field group to one post type unless:
    * It makes sense to have different groups
    * You need a group of fields to be available to more than one post type
* Restrict field groups using the options in the “_Appears on” section_
* Add existing fields to new groups where it makes sense, e.g. an “email” field could be added for contact forms or a “team member” type object, there is no need to make multiple fields that are essentially the same

## Content Templates

Content Templates are used to create sections of a page or a loop element in a view.

* As a general rule you should avoid “hard coding” values into the Content Templates and instead use custom fields or the _wpv-attribute_ shortcode when using the [BDVS Beaver Themer](/wiki/spaces/BDWD/pages/593100813/BDVS+Beaver+Themer) method
* Add CSS to the CSS section and JavaScript to the JS section, unless they are required by more than one element, in which case they should be added to the Customiser
* Do not assign Content Templates to Post Types, use the method described for [Single Templates](/wiki/spaces/BDWD/pages/684883994/Single+Templates) instead

### Beaver Builder Content in Content Templates

It is possible to use Beaver Builder to create a content template, to do this, click the Beaver Builder button in the Content Template editor:  

![](https://bedeviouswebdevelopment.atlassian.net/wiki/download/attachments/703201281/image-20210323-180054.png?version=1&modificationDate=1616522457340&cacheVersion=1&api=v2)

**Warning:** You cannot use Beaver Builder inside Beaver Builder, therefore you cannot use Beaver Builder to create a Content Template and then insert that into a page builder with Beaver Builder. Therefore, the only likely place you can use this method is in an Archive, since those pages are based on Toolset Archives.

## Views

Views are used essentially to create a loop of CPTs and display content for each of them. Typical examples are a latest posts grid showing the last 3 posts from the blog on the home page.

You shouldn’t use a view to display a WordPress Archive, use the method described for [Archives](/wiki/spaces/BDWD/pages/676364289/Archives) instead unless you have a specific reason not to use this.

* Code the entire view into the Loop Editor where you can, only creating a separate content template where:
    * There is a lot of markup for the loop itself as well as the loop item and so splitting it out will help readability
    * The content template will be needed in more places than just this loop
    * You will be using the [_wpv-loop-index_ shortcode](https://toolset.com/documentation/programmer-reference/views/views-shortcodes/#vf-1195331) to display elements differently but they share a common body of content

## Archives

Archives are much like Views but are designed to display the output from the WordPress Loop / Main Query and so we should use the method described in these docs for [Archives](/wiki/spaces/BDWD/pages/676364289/Archives) to make the most of this functionality.