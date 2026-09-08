# Archives

* * *

## Summary

An "archive" here means any page that lists multiple posts of one type: the native
WordPress posts/CPT archive, a taxonomy archive, or a page that's designed to *look* like
a listing even though it isn't a real WP archive URL (e.g. an events page). There are
three approaches below. **Methods 1 and 2 (component/view-based) are the preferred
direction for new work** — pick between them based on whether the listing needs to be a
real WP archive URL or not. Reach for Method 3 (legacy Toolset Archive) only when
maintaining/extending a site that already uses it.

Verified against real sites (2026-09): `homestart-sftp` (`post-archive` component,
targeting the real `archive:post` + `taxonomy:category` locations) and
`the-briny-new-sftp` (`event-rows` / `event-slider` components, on an ordinary Page).
`fortava-sftp` is the reference for the legacy Toolset method.

## Method 1 (preferred): Beaver Themer layout + a `view/` component

Use this for a **real WordPress archive** — a post type's own archive URL, or a taxonomy
archive — where WordPress's native query, rewrite rules, and pagination should keep
working exactly as they otherwise would.

1. Create a Beaver Themer layout (`fl-theme-layout` post type) and set its **Theme
   Builder → Locations** to target the archive(s) it should render on, e.g.
   `archive:post` for the main posts archive, `archive:<cpt-slug>` for a CPT archive,
   `taxonomy:<taxonomy-slug>` for a taxonomy archive. A layout can target more than one
   location at once (homestart's `Post Archive` layout targets both `archive:post` and
   `taxonomy:category`).
2. Build the layout in Beaver Builder as normal. It's fine — and common — to stack more
   than one row: an intro/hero row built from ordinary modules or another component
   (homestart's layout has a `slider-type-1` row before the loop), followed by a row
   containing the loop component.
3. For the loop itself, create (or reuse) a **`view/` component** (see root `CLAUDE.md`
   component architecture). Its `view.php` should be exactly:
   ```php
   <?php
   global $wp_query;

   $query = $wp_query;
   ```
   This hands the component the real main query WordPress already built for that
   archive/taxonomy request — so search, pagination, and any query-string filtering the
   theme adds all keep working with zero extra code in the component. (Confirmed example:
   `homestart-sftp/components/view/post-archive/`.)
4. `template.php` loops `$query` exactly like any other `view/` component — see
   "Building the loop markup" below.
5. Place the component on the layout via the usual `bdvs-content-template` module
   (`select_template` = the component's folder name).

### Setting the archive slug

For a CPT, edit the post type under **Toolset → Post Types** to set a friendlier archive
slug than the default post-type name. This step doesn't apply to WordPress Posts — see
"Posts archive" below.

### Don't build a separate Toolset Archive/View for this

Once a Beaver Themer layout is targeting the archive location, there's no need for a
parallel Toolset Archive or View doing the same query — that's the legacy method
(Method 2) and the two shouldn't be combined for the same listing.

## Method 2 (preferred, for non-archive listings): a Page + a `view/` component with its own query

Use this when the listing **doesn't need to be a real WP archive URL** — no native
pagination or archive rewrite semantics required, just "show these posts of type X in
this layout." This is the right fit for a small, fixed-ish set (an events page, a
locations page) built as an ordinary Page.

This is the same pattern the existing Toolset guidance already establishes for the Posts
archive (see below) — a real Page carries the content, rather than fighting WordPress's
own archive slug/rewrite handling — generalised here to any post type via a component
instead of Toolset's native View/Archive editor.

It also covers a single-item teaser, not just a listing — e.g. "show the most recent
Case Study" as one card on the home page (`posts_per_page => 1`). Before building the CPT
for a case like this, don't assume it needs a single template just because it's a
"content" CPT — see the note under
[Custom Post Types](../General/Toolset.md#custom-post-types) — and see
[Single Templates: prefer WordPress's native fields](Single%20Templates.md#prefer-wordpresss-native-fields-before-creating-custom-ones)
for how that decision changes whether the teaser uses full content vs excerpt, and
whether its CTA needs its own link field at all.

1. Create the Page normally (title + slug you want the listing to live at).
2. Create a `view/` component whose `view.php` builds its own `WP_Query`, e.g.
   (`the-briny-new-sftp/components/view/event-rows/view.php`):
   ```php
   <?php
   $post_type = 'event';
   $posts_per_page = -1;

   $args = array(
     'post_type'      => $post_type,
     'posts_per_page' => $posts_per_page,
     'post_status'    => 'publish',
     'orderby'        => 'menu_order',
     'order'          => 'ASC',
   );

   $query = new WP_Query($args);
   ```
   Using `posts_per_page => -1` with `orderby => menu_order` is typical here since the
   point is usually "show everything, admin-ordered" rather than paginate — if the CPT
   needs manual ordering in the admin, enable **hierarchical** on the post type (see
   `General/Toolset.md`) so the Simple Page Ordering drag-and-drop is available.
3. Drop the component onto the Page via Beaver Builder as normal — it's just another
   `bdvs-content-template` module, no Themer layout involved at all.
4. It's fine to have more than one component reading the same post type for different
   presentations on the same page (briny's `What's On` page combines `event-slider` for a
   top summary carousel with `event-rows` for the full stacked listing below it — both
   query `event` independently via their own `view.php`).

### Posts archive (WordPress Posts, not a CPT)

WordPress always uses the post-type slug for a CPT archive, but for standard Posts the
slug is controlled differently: create a Page with the desired title/slug and set it as
the blog page under **Settings → Reading → Posts Page**. Either archive method above
still applies for the *content* of that page — Method 1 (a Themer layout targeting
`archive:post`, as in homestart's `Post Archive` layout) is preferred since a Posts Page
is still a real WP archive query under the hood.

## Building the loop markup (`template.php`)

Whichever method supplies `$query`, `template.php` loops it the same way as any other
`view/` component — standard WP loop functions inside `BDVS LOOP START`/`END` markers:

```html
<div class="tb-event-rows-container">

  <!-- BDVS LOOP START -->

  <div class="tb-event-row" id="<?php echo esc_attr(get_post_field('post_name', get_the_ID())); ?>">
    <h2><?php the_title(); ?></h2>
    <div><?php echo apply_filters('the_content', get_the_content()); ?></div>
  </div>

  <!-- BDVS LOOP END -->

</div>
```

A few things worth carrying over from the real examples:

- Use `get_the_ID()` / `get_post_field()` / `get_post_meta()` rather than template-tag
  shortcuts that assume the global post is already set up correctly for a non-standard
  query.
- If the loop needs to vary styling by position (alternating row background/direction,
  say), key off `$query->current_post` inside the loop rather than hardcoding per-item
  markup.
- If another component on the same page needs to link into a specific loop item (e.g. a
  slider's "Read more" button jumping to the matching row further down the page), give
  each loop item an anchor derived from the post slug (`id="<?php echo esc_attr(get_post_field('post_name', get_the_ID())); ?>"`)
  and link to `#<slug>` from the other component.

## Archive headers/footers that aren't part of the loop layout itself

If an archive needs intro content that doesn't belong inside the loop component (a
one-off hero image/title that isn't itself a listing), the older approach of a separate
Beaver Themer "Part" assigned to appear after the header, scoped to just that archive,
is still valid and simpler than building it into the loop layout — see `fortava-sftp`'s
`Post Archive Hero` / `Job Vacancy Archive Hero` layouts. It's acceptable to hand-enter
that content directly in Beaver Builder rather than pull it from a field, since archive
titles/intros aren't easy to source generically.

## Method 3 (legacy): native Toolset Archive

Older projects (`fortava-sftp` and earlier) use Toolset's own Archive editor rather than
a component: a Toolset Archive owns the query, loop markup, search, and pagination
directly, with a separate Beaver Themer "Part" (as above) supplying any hero/intro
content before it. This still works and there's no need to migrate an existing site's
working archive to Method 1 just for its own sake — but don't build new archives this
way; use Method 1 (or Method 2 above, for a non-archive listing page) instead.

If maintaining a legacy Toolset Archive:

* The query/pagination/search behaviour is Toolset's, not something to hand-roll.
* Avoid hardcoding values into the Archive's Content Template — use custom fields or the
  Toolset Views shortcodes instead (see `General/Toolset.md`).
* A Content Template *can* use Beaver Builder internally here (Archives are one of the
  few places this is possible, since Beaver Builder can't nest inside Beaver Builder) —
  but a component under `components/view/` is the more maintainable, reusable
  alternative going forward.

---
