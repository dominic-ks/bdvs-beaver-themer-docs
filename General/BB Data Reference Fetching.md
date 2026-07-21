# BB Data Reference Fetching

* * *

## Summary

This document is the step-by-step checklist for pulling real Beaver Builder / Beaver
Themer draft data from a live site into the reference library at
`bdvs-bb-data-reference/` (see that directory's `README.md` for what the library is and
how it's organized). It extends the SSH/wp-cli process documented in the root
`CLAUDE.md` ("Editing BB Pages Directly via Post Meta") — read this after that, not
instead of it.

This is a **read-only** process end to end. Nothing here writes to any post meta; it only
fetches and saves. The draft-only / never-touch-`_fl_builder_data` discipline from the
root CLAUDE.md still governs any *write* work that uses this data afterwards.

## Checklist

1. **Resolve connection details.** Read `sftp-themes/<project>/.vscode/sftp.json` for
   `host`, `port`, `username`, `remotePath`, `privateKeyPath`. The WordPress root is the
   parent of `remotePath` (which points at `wp-content/themes/bb-theme-child`).

2. **Stage the SSH key.** `privateKeyPath` is often a Windows-mounted path
   (`/mnt/c/Users/...`) with overly-open permissions. Copy it to a scratch location and
   `chmod 600` it — SSH refuses group/world-readable keys. Delete the scratch copy when
   done with the session.

3. **Find candidate content.**
   - For a **component** example: either ask the user which page uses it, or find one
     yourself — `wp post list --post_type=page --fields=ID,post_title,post_name` to see
     what pages exist, then either check the live rendered HTML for the component's CSS
     prefix class name (cheap, no SSH needed — just fetch the public URL), or fetch a
     likely page's draft and grep it for `'select_template' => '<component-name>'`.
   - For a **full page** example: pick one whose section mix is representative — a
     homepage or a content-rich landing page, not a thin one-liner page.
   - For a **themer part**: list `fl-theme-layout` posts —
     `wp post list --post_type=fl-theme-layout --fields=ID,post_title,post_status`. This
     is the post type Beaver Themer layouts live under (header, footer, archive
     templates, single templates, and generic "parts" are all this one post type,
     differentiated by title/targeting, not by post type).

4. **Fetch the draft.**
   ```bash
   ssh -i <staged-key> -p <port> <user>@<host> \
     "cd <wp-root> && wp post meta get <post_id> _fl_builder_draft"
   ```
   Falls back to `_fl_builder_data` **for reading only** if a post has no draft (e.g. it
   was never opened in the builder since last published) — never write to that key.
   wp-cli's default `post meta get` output is `var_export`, not raw `serialize()` — the
   result is a literal, directly-readable PHP expression.

5. **Isolate the node(s) you actually want.**
   - For a **full page or themer part**, the whole fetched array is what you want — no
     isolation needed.
   - For a **single component instance** inside a larger page, don't hand-parse the
     `var_export` text to find matching braces — isolate it server-side instead, where
     PHP does the parsing for you:
     ```bash
     ssh -i <staged-key> -p <port> <user>@<host> \
       "cd <wp-root> && wp eval '
     \$draft = get_post_meta(<post_id>, \"_fl_builder_draft\", true);
     foreach (\$draft as \$id => \$node) {
         if (isset(\$node->settings->select_template) && \$node->settings->select_template === \"<component-name>\") {
             echo \$id . \"\n\";
             var_export(\$node);
             echo \"\n\";
         }
     }
     '"
     ```
     This prints every module node using that component, each with its real node ID —
     pick whichever instance has the most representative (non-blank, realistic) content.
     A benign `Unsuccessful stat on filename containing newline...` line may appear first
     from the server's PHP CLI wrapper reacting to the multi-line inline script — that's
     noise from the invocation, not part of the data; drop it when saving.

6. **Save into the reference tree with a metadata header.** Prepend a `//`-commented
   header (source project, source post title/ID/URL, node ID(s) where relevant, fetch
   date, and the command used) before the raw payload. See any existing file under
   `bdvs-bb-data-reference/` for the exact format. Save as `.php.txt`, not `.php` — these
   are reference fragments to copy from, not standalone executable files.

7. **Clean up.** Delete the scratch copy of the private key.

## What this checklist deliberately does not cover

- Writing draft data back to a site — that's the existing "Making edits" / "Writing
  back" sections of the root `CLAUDE.md`, unchanged by this doc.
- Beaver Themer location/targeting rules (which pages a layout applies to) — not stored
  in `_fl_builder_draft`, and not something this checklist fetches. Flagged as a gap in
  `bdvs-bb-data-reference/themer-parts/NOTES.md`.
