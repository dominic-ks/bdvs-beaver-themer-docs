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
* If the CPT is for managing data and will not have an archive or public facing single page disable these options (e.g. testimonials), if it will have a public facing single page (e.g. case studies), enable them:
    * Rewrite
    * Allow permalinks to be prepended with front base
    * **Don't infer "will have a single page" from the CPT's name or content type alone** — a case study, event, or similar CPT is just as often used purely as a data source for a teaser/listing elsewhere (e.g. "most recent case study" on the home page) with no single template ever built. Confirm with the actual site plan (existing pages, what's linked from "read more"-style buttons today) before enabling these.
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

Toolset's modern relationships (the "m2m" engine — `toolset_is_m2m_enabled` option) store
relationships in three custom DB tables, not an option: `{prefix}toolset_relationships`
(one row per relationship), `{prefix}toolset_type_sets` (maps each relationship side's
`set_id` to a post-type slug — **every relationship side gets its own fresh `set_id` row,
even when the same post type is reused across multiple relationships**), and
`{prefix}toolset_associations` (the actual parent↔child links between real posts).
Confirmed by reading a real, working relationship on `fortava-sftp`
(`team-member-care-home`, a genuine many-to-many) before building a first one elsewhere —
mirror an existing relationship's row the same way other Toolset registrations in this
repo are mirrored, rather than guessing the ~15 columns blind.

**Building one via SSH + wp-cli** (confirmed end-to-end on `connected-by-fusion-sftp`,
`case-study` ↔ `service`): insert two `toolset_type_sets` rows (one per side, fresh
`set_id`s), then one `toolset_relationships` row referencing those `set_id`s as
`parent_types`/`child_types`. For an ordinary many-to-many: `cardinality_parent_max` /
`cardinality_child_max` = `-1`, `ownership` = `0`, `is_distinct` = `1`, `origin` =
`'wizard'`, `role_name_*`/`role_label_*` blank, `needs_legacy_support` = `0`,
`is_active` = `1`, `autodelete_intermediary` = `1`.

**Reading a relationship's related posts** (front end, in a component or a custom
shortcode): `toolset_get_related_posts( $post_id, '<relationship-slug>', 'child' )` (or
`'parent'`) — this is the same PHP API whether it's a genuine content relationship
("Services Used" on a Case Study) or the repeating-field-group construct below, since a
repeating group's items are really just related child posts.

### Repeating field groups are relationships in disguise

A Toolset "repeating field group" (the `+ Add` repeater UI on a CPT's edit screen,
letting an editor add any number of value-sets like stat tiles or FAQ items) is **not** a
special field type — it's a hidden, non-public CPT (registered in `wpcf-custom-types`
with `is_repeating_field_group => true`, `public => false`, `show_ui => false`, supports
`post_title`/`author`/`custom-fields` only) whose instances are related to the real
parent post via a **fixed one-to-many relationship**: `cardinality_parent_max = 1`
(each repeat item belongs to exactly one parent), `cardinality_child_max = -1`,
`origin = 'repeatable_group'`, `role_name_parent/child/intermediary` =
`'parent'`/`'child'`/`'association'`, `role_label_*` = `'Parent'`/`'Child'`/`'Parents'`/
`'Children'`, `is_distinct = 0`. Confirmed against `fortava-sftp`'s `home-facilities`
repeating group. The group's own sub-fields are ordinary `wpcf-fields` entries
(`meta_type => 'postmeta'` on the hidden CPT's own posts), and its `wp-types-group` field
group post uses `types_field_group_purpose => 'for_repeating_field_group'` and
`_types_repeatable_field_group_post_type => '<hidden-cpt-slug>'` instead of the normal
`_wp_types_group_post_types` key.

**Wiring that's easy to miss when building one by hand** (confirmed against Creative
Clique's working `feature-sections` group and Types 3.6.3's own lookup code, which finds
a group's items by the field group post's own slug):

- The field group post's `post_name`, the hidden CPT slug, and the relationship `slug`
  must all be **the same string** (e.g. `feature-sections` / `feature-sections` /
  `feature-sections`).
- The field group post's `post_status` is `hidden`, not `publish`.
- The parent's own field group lists the repeating group as a pseudo-field
  `_repeatable_group_<rfg field group post ID>` in its `_wp_types_group_fields` (e.g.
  `,client-logo,...,_repeatable_group_2841,`). This is what puts the repeater on the
  parent's edit screen.
- Each item post carries `toolset-post-sortorder` meta (1, 2, 3…), which holds the
  editor's drag-and-drop order.

`connected-by-fusion-sftp`'s Case Study "Results Stats" group (`result-stat`) was built
before this was known and gets all three slug/status/listing points wrong (group slug
`result-stat-fields`, status `publish`, relationship `case-study-result-stat`, not listed
in the Case Study Fields group). That's probably why `toolset_get_related_posts()`
returns nothing for it, and why `cbf_get_related_post_ids()` in that theme's
`functions.php` reads the association tables directly instead. **That helper has a
bug:** `toolset_associations.parent_id`/`child_id` hold
`toolset_connected_elements.group_id`, not that table's `id`. The helper joins on `id`,
which only works while the two numbers happen to be equal. They stop matching once rows
are deleted, as confirmed on CBF in October 2026. Use `toolset_get_related_posts()` on a
correctly wired group instead of reading the tables. If you must read them, join through
`group_id`.

Two more hand-registration traps:
- `toolset_type_sets.type` is `varchar(20)`. A longer slug makes the insert fail
  silently, and the relationship then "doesn't exist" to the API.
- WordPress caps post type names at 20 characters anyway, so keep repeating group slugs
  within that.

Repeating groups can be **nested** (a repeating group inside a repeating group's own
field group, listed there by the same `_repeatable_group_<id>` pseudo-field); the
nested group's relationship simply has the outer group's CPT as its parent type. See
[Flexible Layouts](../Elements/Flexible%20Layouts.md).

Rendering a repeating group's items in a component: rather than guessing Types' own
front-end repeater shortcode syntax, look up the item post IDs through the relationship,
sort them by `toolset-post-sortorder`, then read each item's fields with plain
`get_post_meta()`. On `connected-by-fusion-sftp` that's `cbf_get_rfg_item_ids()` in
`functions.php`, used by the Service sections. The older `cbf_case_study_result_stats()`
uses the unsorted `cbf_get_related_post_ids()` directly.

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

Archives display the output from the WordPress Loop / Main Query. Native Toolset
Archives are a legacy approach here — see [Archives](../Elements/Archives.md) for the
preferred method (a Beaver Themer layout targeting the archive/taxonomy location, with
the loop rendered by a `view/` component) and when the legacy Toolset method is still
appropriate.