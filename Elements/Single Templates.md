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
   **Theme Builder → Locations** target the post type (all posts of that type, or a
   specific post if it's a one-off template).
4. **Beaver Builder**: build the layout as normal, pulling in the post's own fields (see
   below) rather than typing content in directly (typing directly is fine for one-off
   pages, but a single template runs across every post of that type, so its content
   must come from the post being rendered).

### The two meta keys that actually drive this (correcting the UI's own naming)

Confirmed against a real, rendering layout (`homestart-sftp`'s `Single Project`, ID 893,
`post:project`) versus a from-scratch attempt on `connected-by-fusion-sftp` that matched
the CPT (visible as a `fl-theme-builder-` body class fragment) but rendered no content
until this was fixed:

- `_fl_theme_builder_locations` (serialized array of strings) — despite the admin UI
  calling this "Locations" with entries that read like `singular:<cpt-slug>`, the value
  actually stored is **`post:<cpt-slug>`** (e.g. `post:project`, `post:service`), not
  `singular:<cpt-slug>`. Header/footer layouts use a different vocabulary entirely
  (`general:site`); archive layouts use `archive:<post-type>` / `taxonomy:<taxonomy>`.
- `_fl_theme_layout_type` (a plain string: `singular`, `header`, `footer`, `archive`,
  etc.) — a **separate** meta key that also has to be set. Without it, Beaver Themer
  still matches the location internally (the post-type fragment of the
  `fl-theme-builder-*` body class renders) but never actually injects the layout's
  content into the page — the giveaway is a body class like `fl-theme-builder-` /
  `fl-theme-builder--<layout-slug>` (empty type segment) instead of the expected
  `fl-theme-builder-singular fl-theme-builder-singular-<layout-slug>`.

Both keys need to be present and correct; missing either one produces a state that looks
plausible (post type matched, no PHP errors) but silently renders nothing. When converting
an existing published page into a layout via direct postmeta edits (see the root
`CLAUDE.md`'s "Editing BB Pages Directly via Post Meta" section) rather than building one
fresh through the Beaver Themer admin UI, both of these have to be added by hand — they
don't get created automatically just by changing the post's `post_type`.

## Three ways to pull field data into the layout

Pick per-field based on the module involved and whether the connect UI can express the
value on its own.

### 1. A native BB module, connected via Beaver Themer

For anything a stock Beaver Builder module already renders well (a Photo module for the
featured image, a Rich Text module for the main body copy), drop that module onto the
layout and use Beaver Themer's field-connection UI on the module (the small connect
icon in the module's settings) to point its content at a post/custom field, instead of
typing static content into it. This is the more common case for simple fields — confirmed
on `homestart-sftp`'s `Single Project` layout, where the body copy and two images are
plain `rich-text`/`photo` modules connected this way, with no custom component involved.

### 2. A `bdvs-content-template` component field, connected the same way (preferred over shortcodes)

**Prefer this over option 3 whenever a field just needs one dynamic value** (a title, a
featured image, an excerpt) — including fields inside a `bdvs-content-template` component,
which is easy to miss since those modules look like one big custom settings blob rather
than a set of ordinary BB fields. The `bdvs-content-template` module (see
`bdvs-beaver-themer/inc/modules/bdvs-content-template/bdvs-content-template.php`,
`get_connection_types()`) registers every `string`/`url`/`color`/`photo`/`custom_field`
attribute as a connectable field, so the same connect-icon UI from option 1 works directly
on a component's own attributes (a hero's Title field, its Background Image field, etc.) —
no template edits needed. Confirmed live on `homestart-sftp`'s `Single Project` layout
(`components/view-template/hero`-style slide fields): the connection is stored per field as
a JSON string in the module's `settings.connections` array, e.g.
`'hsp-slide-title' => '{"object":"post","property":"title","field":"text","settings":null}'`
(other `property` values seen: `featured_image_url`, `excerpt`, `custom_field` with a
`settings.key` of the target `wpcf-*` meta key) — the field's own literal value is left
alone as an editor-preview placeholder; the connection is what drives the live render.
Applied on `connected-by-fusion-sftp`'s `service` CPT single template (converted from the
old Services page), where the `hero` component's `cbf-hero-title` field is connected to
Post Title this way rather than typing `[wpv-post-title]` into the field.

### 3. A `bdvs-content-template` component, using Types/Views shortcodes directly in markup

Fall back to this only when the connect UI genuinely can't express the value — real PHP
logic, a loop, a taxonomy pull, or several fields combined into one piece of markup that
isn't just "this field equals that post property". When the layout needs bespoke markup
around several fields together (a card-style header combining title + two custom fields +
a CTA button, say), build a normal `view-template/` component and pull field values with
real shortcodes written directly into `template.html` — these run against the actual post
being rendered, they are not placeholders:

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
