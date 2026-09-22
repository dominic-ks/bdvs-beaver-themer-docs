# Global Rows & Modules

* * *

## Summary

A **Global** row, column, or module is native Beaver Builder functionality: the
content — every child row/column-group/column/module underneath it, and their
settings — lives in one place, and every page that uses it renders that same live
content. Editing it anywhere (in the BB editor, on any page it's placed on) updates it
everywhere it's placed. This is different from an ordinary **Saved** row/module (BB's
"save to reuse" option, not global) — a Saved element is just a copy you can drop in
again; each instance then drifts independently.

Use Global for content that genuinely must be identical everywhere it appears — client
logo strips, testimonials sliders, footer CTA bands. See
[Beaver Builder.md](Beaver%20Builder.md)'s "Saved and Globally Saved Rows, Columns and
Modules" section for the standard in-editor workflow (right-click the element → Save,
tick Global). That's the right approach when you're building the element for the first
time.

This document covers the other case: **converting content that already exists on a live
page into a global row/module**, via SSH + wp-cli, without rebuilding it by hand in the
editor. Confirmed end-to-end: `fortava-sftp` has two live row-level globals ("Testimonials
Row", "Our Homes Row") that were inspected as reference; a module-level global ("Client
Logos") was built and verified working on `connected-by-fusion-sftp`.

## How it actually works (from the BB core source)

Reverse-engineered from `wp-content/plugins/bb-plugin/classes/class-fl-builder-model.php`
(`save_node_template()` / `apply_node_template()`) on a live site — don't trust this
summary alone if BB core changes; re-check the source if something here stops matching
reality.

- The global's content lives as a post of type `fl-builder-template`, in that post's own
  `_fl_builder_data` (published) / `_fl_builder_draft` (draft) meta — same two-key system
  as any page. This post is the single source of truth.
- Taxonomy `fl-builder-template-type` is set to the root node's `type` — `row` if you're
  globalising a whole row, `module` if just a module. (Also has a `layout` term, unrelated
  — that's for full saved-page layouts, not global rows/modules.)
- Every page that places the global gets **only the root node** copied into its own
  `_fl_builder_draft` (and, once published, `_fl_builder_data`) — not the row's children.
  A row's column-group/column/module tree lives solely in the template post; a page never
  duplicates it. The root node BB copies onto each host page carries a full copy of
  `settings` too (so the editor/preview don't need a round-trip to render it), plus four
  extra keys that mark it as a global reference:
  ```php
  'global'             => true,           // was `false` on an ordinary node
  'template_id'         => '<opaque-id>',  // same value on every placement + the template's own copy
  'template_node_id'    => '<node-id>',    // the root node's own ID *inside the template post's data* — same on every placement
  'template_root_node'  => true,
  ```
  `template_id` is a separate generated ID (12-char lowercase alphanumeric, matching
  regular node ID format), stored on the template post as `_fl_builder_template_id`. It is
  **not** the template post's WordPress post ID and not any node's own `node` key — it's a
  standalone opaque identifier that ties every placement back to the same template post.
- Each page's own copy of the root node still gets a **fresh, unique `node` id** per
  placement — don't reuse the same node id across pages, and don't reuse the template
  post's internal copy's node id either.
- Template post meta, in full:
  ```php
  _fl_builder_enabled          = true
  _fl_builder_template_global  = true
  _fl_builder_template_id      = '<the opaque template_id above>'
  _fl_builder_data_settings    = (object) [ 'css' => '', 'js' => '' ]
  _fl_builder_template_module_type = '<module's settings->type>'   // module-level only, e.g. 'bdvs-content-template'
  _fl_builder_data             = <root node + its full child tree, keyed by node id>
  _fl_builder_draft            = <same, kept in sync>
  ```

## Doing it via SSH + wp-cli

This extends the root `CLAUDE.md` section "Editing BB Pages Directly via Post Meta" — read
that first. Same tooling (`wp post meta get` / `wp eval-file`), same discipline.

1. **Pick row-level vs module-level.** Fetch the draft of a page that has the content you
   want to globalise (see
   [BB Data Reference Fetching.md](BB%20Data%20Reference%20Fetching.md) for the fetch/isolate
   steps) and look at what's actually inside the row:
   - If the row's column contains **only** the element you're globalising (nothing else
     sharing that column or row) — go row-level. Copy the row → column-group → column →
     module chain into the template. (`fortava-sftp`'s Testimonials Row and Our Homes Row
     are both this case — each is a dedicated row with a heading + one module, and nothing
     page-specific alongside it.)
   - If the element shares its row/column with other, page-specific content (a heading
     that differs per page, other unrelated modules) — go module-level, globalising just
     the module node. Going row-level here would either strip that other content off every
     page but the source, or force it onto pages that never had it — neither is a "sync the
     shared content" change, it's a silent content change nobody asked for. (This was the
     case on `connected-by-fusion-sftp`: the Contact page's row had a "Proud To Work With"
     heading alongside the logos module that Home's row didn't have.)
   - If you're not sure which one the task actually wants, ask — it's a real content
     decision, not a technical detail.

2. **Generate the IDs you'll need.** Four 12-character lowercase-alphanumeric strings,
   each unique against every existing node ID in every draft file you're touching (grep
   for collisions before using one):
   - `TEMPLATE_ID` — the opaque cross-placement identifier (→ template post's
     `_fl_builder_template_id` meta, and every placement's `template_id` field).
   - `TEMPLATE_NODE_ID` — the root node's own id *inside the template post's data* (→
     every placement's `template_node_id` field, and the `node` key of the template post's
     own copy).
   - One fresh `node` id **per page** you're placing the global on (these are NOT shared
     across pages).

3. **Build the template post's node data.** Take the existing node (and, for a row, its
   full child tree) from whichever page currently has the best/most current copy of the
   content, and adjust:
   - Root node's own `node` key → `TEMPLATE_NODE_ID`; `parent` → `NULL`; `position` → `0`.
   - Append to the root node (after its existing `version`/whatever trailing keys):
     `'global' => true, 'template_id' => '<TEMPLATE_ID>', 'template_node_id' =>
     '<TEMPLATE_NODE_ID>', 'template_root_node' => true, 'dynamic' => false,` — replacing
     whatever `'global' => false` was there before.
   - Non-root children (row-level only) keep their own existing node ids and parent chain
     as-is — no global/template_* keys on them, only the root gets those.

4. **Create the template post**, `wp eval-file` a PHP wrapper (`php -l` it first, same as
   any other draft edit):
   ```php
   $post_id = wp_insert_post( array(
       'post_title'     => '<Name>',                 // e.g. "Client Logos"
       'post_type'      => 'fl-builder-template',
       'post_status'    => 'publish',
       'ping_status'    => 'closed',
       'comment_status' => 'closed',
       'post_excerpt'   => '',
   ) );
   wp_set_post_terms( $post_id, '<row|module>', 'fl-builder-template-type' );
   update_post_meta( $post_id, '_fl_builder_template_id', '<TEMPLATE_ID>' );
   update_post_meta( $post_id, '_fl_builder_enabled', true );
   update_post_meta( $post_id, '_fl_builder_template_global', true );
   update_post_meta( $post_id, '_fl_builder_data_settings', (object) [ 'css' => '', 'js' => '' ] );
   // module-level only:
   update_post_meta( $post_id, '_fl_builder_template_module_type', '<module settings->type>' );
   update_post_meta( $post_id, '_fl_builder_draft', $data );   // draft only — see below
   ```

   **Stop at `_fl_builder_draft` — do not also write `_fl_builder_data` here**, even though
   it's a brand-new post nothing else references yet. It's still a live-site write to a
   `_fl_builder_data` key, which is exactly what the draft-only discipline in the root
   `CLAUDE.md` exists to prevent, and this repo's own tooling will refuse it for that
   reason. The tradeoff: the global template post won't actually render (its published
   data is empty) until a human opens it in the BB editor and clicks **Publish** once. Flag
   that as the one remaining manual step — it's the same human-review gate every other
   draft-only edit in this workflow already relies on, just applied to the template post
   instead of a page.

5. **Point each page at it.** In each page's `_fl_builder_draft`, replace the existing
   local node (root of the row, or the module) with a new node: fresh unique `node` id from
   step 2, same `parent`/`position` as before (so it stays in the same place in the page),
   full copy of the root node's `settings`, and the same four `global`/`template_id`/
   `template_node_id`/`template_root_node` keys as the template post's copy — same
   `TEMPLATE_ID`/`TEMPLATE_NODE_ID` values on every page. Write back via `wp eval-file` to
   `_fl_builder_draft` only, same as any other draft edit.

6. **Verify, then hand off.** Re-fetch each post's meta to confirm the write landed
   (`grep -c "'global' => true"` is a quick sanity check). Delete any temp `.php` scripts
   from the webroot. Then tell whoever's reviewing: publish the template post first, then
   publish each page — in the BB editor, same as any other draft change.

## Un-globalising / editing afterwards

Everyday edits (e.g. swapping the logos) only need the template post's own draft updated
and published — the pages referencing it don't need to be touched at all, since they only
store the root node's cached settings as a snapshot; the actual render pulls from the
template post's published data. To remove a global (unlink it back to a normal local
element, or delete it entirely), do that through the BB editor rather than by hand — BB's
`unlink_global_node_template_from_post()` / `delete_global_node_template_from_all_posts()`
logic does bookkeeping across every page that references the template that isn't worth
replicating by hand over SSH.
