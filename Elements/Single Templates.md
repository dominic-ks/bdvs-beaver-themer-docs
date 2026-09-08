# Single Templates

* * *

## Summary

Single templates display content for a single post of a CPT (or, less commonly, a
WordPress Page/Post). They're built with Beaver Themer, targeting a specific post type,
and pull real content from that post via Beaver Themer's own field-connection system —
not via component "attributes" (that mechanism is for example/variation data at build
time, not live per-post field data — see "Don't confuse this with component attributes"
below).

The general setup:

1. **Toolset → Post Types**: create the CPT if it doesn't exist yet (see
   [Custom Post Types](../General/Toolset.md#custom-post-types)).
2. **Toolset → Custom Fields**: create one field group per CPT, named `<CPT> Fields`
   (e.g. `Event Fields`), restricted to that post type via **Appears on**
   (see [Custom Fields](../General/Toolset.md#custom-fields)). This is a real,
   consistently-followed convention — confirmed on `the-briny-new-sftp` (`Event Fields`
   group → `event` CPT, fields stored as `wpcf-event-*` post meta).
3. **Beaver Themer**: create an `fl-theme-layout` layout, and under
   **Theme Builder → Locations** target the post type
   (e.g. `singular:<cpt-slug>` for all posts of that type, or a specific post if it's a
   one-off template).
4. **Beaver Builder**: build the layout as normal, pulling in the post's own fields (see
   below) rather than typing content in directly (typing directly is fine for one-off
   pages, but a single template runs across every post of that type, so its content
   must come from the post being rendered).

## Two ways to pull field data into the layout

Both are used, side by side — pick per-field based on whether a native BB module already
does the job, or the value needs to be embedded inside custom component markup.

### 1. A native BB module, connected via Beaver Themer

For anything a stock Beaver Builder module already renders well (a Photo module for the
featured image, a Rich Text module for the main body copy), drop that module onto the
layout and use Beaver Themer's field-connection UI on the module (the small connect
icon in the module's settings) to point its content at a post/custom field, instead of
typing static content into it. This is the more common case for simple fields — confirmed
on `homestart-sftp`'s `Single Project` layout, where the body copy and two images are
plain `rich-text`/`photo` modules connected this way, with no custom component involved.

### 2. A `bdvs-content-template` component, using Types/Views shortcodes directly in markup

When the layout needs bespoke markup around several fields together (a card-style header
combining title + two custom fields + a CTA button, say), build a normal
`view-template/` component and pull field values with real shortcodes written directly
into `template.html` — these run against the actual post being rendered, they are not
placeholders:

- `[types field="field-slug"][/types]` — a Toolset Types custom field (the CPT's own
  fields from its `<CPT> Fields` group)
- `[wpv-post-title]`, `[wpv-post-body]`, `[wpv-post-featured-image]`,
  `[wpv-post-date]`, `[wpv-post-taxonomy type="..."]` — built-in Toolset Views post
  shortcodes for standard WP fields
- `get_the_title()`, `get_the_post_thumbnail_url()`, `get_post_meta(get_the_ID(), 'wpcf-<field-slug>', true)`,
  etc. — plain WP functions, used the same way as in a `view/` component's
  `template.php`, when a component needs PHP logic around the value rather than just
  echoing it (custom template functions, not raw shortcodes, are how `view/` components
  built for this purpose access fields — see `the-briny-new-sftp/components/view/event-rows/template.php`
  for the pattern, e.g. `get_post_meta(get_the_ID(), 'wpcf-event-sub-heading', true)`)

## Prefer WordPress's native fields before creating custom ones

When registering a CPT's `<CPT> Fields` group (or when converting an existing
hand-typed component into a CPT — see below), don't reach for a custom field where a
native WP field already does the job:

- **Image** → the post's **featured image**
  (`get_the_post_thumbnail_url()` / `[wpv-post-featured-image]`), not a custom
  `photo` field, unless the post genuinely needs more than one image.
- **Title** → the post title, not a custom `heading` field.
- **Main body copy** → the post content (or excerpt — see below), not a custom
  `description`/`editor` field.

Reach for a real custom field only for data that isn't one of WordPress's built-in post
attributes (a case study's client name, an event's start time, a job vacancy's salary
band, etc).

### Content vs excerpt, and where "read more" links go

Whether the teaser/card shows full content or an excerpt depends on whether the CPT has
a single template at all (see the "don't infer a single page" note under
[Custom Post Types](../General/Toolset.md#custom-post-types)):

- **No single template** — there's nowhere else for the reader to go, so use the full
  post content (`get_the_content()` / `[wpv-post-body]`).
- **Has a single template** — the teaser should use the excerpt
  (`get_the_excerpt()` / `[wpv-post-excerpt]`, or a manually-set excerpt), with the full
  content reserved for the single page.

Either way, a "read more"/"view case study"-style CTA on a teaser should link via
`get_permalink()` / `[wpv-post-url]`, not a separately hand-typed link field — the link
*is* the post, so it shouldn't be data entered a second time. If a hand-typed component
being converted to a CPT has more than one CTA button, check what each one actually
links to before assuming both need to become fields: one is often just "go to this
post" (`get_permalink()`, no field needed) while only a genuinely separate destination
(e.g. a fixed "Contact Us" link reused across every card) needs its own field — or may
not need to be per-post data at all if it's the same link on every card.

### Don't confuse this with component attributes

`view-template/` components also support build-time **attributes**
(`<!-- wpv-attribute name="..." ... -->` placeholders filled in per-instance when the
component is dropped onto a page, documented via each component's own `examples.json`).
That mechanism is for components used as **general-purpose, hand-configured content
blocks** (a CTA banner where you type the heading once), not for single templates, where
every field must come from whichever post is currently being rendered — a wpv-attribute
placeholder has no idea which post that is. If a component is built specifically to be
used inside a single template, its fields should be real Types/Views shortcodes or PHP
calls as above, not `wpv-attribute` placeholders.

---
