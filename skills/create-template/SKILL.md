---
name: create-template
description: Design a HitApply résumé or cover letter template (LaTeX) with the user and save it to their HitApply account. Use when the user says "make me a résumé template", "a résumé like Jake's but more creative", "a two-column/modern/colourful template", "change my HitApply template's look", or wants a new cover letter design. Needs the HitApply MCP with write access.
---

# Create a HitApply template

A template is the **look** only. HitApply fills in the user's own content, so you never write their
name, jobs or bullets into it. You design the look; HitApply checks the template, previews it and
stores it, with the `save_template` tool.

The rules a template must follow are at https://hitapply.app/help/templates/reference. Read it
before your first edit, and again whenever a check fails.

## 1. Start from a template
- Pick the closest starting point and call `save_template(kind, start_from=...)`:
  - **resume**: "Jake's Resume — Classic" (the Jake's layout), "Jake's Resume — Company First",
    "Modern Resume".
  - **cover**: "Jake's Letter", "Minimal Letter", "Modern Letter", "Corner Lines Letter".
  - **One of the user's own**: pass its name. A wrong name returns the valid ones.
- `forbidden`: the connection is read-only. Tell the user to reconnect HitApply with changes
  allowed, then stop.

## 2. Change the look
Edit the returned `.tex` to match what the user asked for: fonts, colours, spacing, headings,
rules, the order of fields. Keep it **one file** with standard TeX Live packages only (no images,
custom fonts or `.sty` files). Keep every macro, even ones this design rarely shows, and keep the
metadata block (give it the new `name`).

## 3. Check and look
- Call `save_template(kind, source=...)`. Nothing is saved yet. You get `pages`, any `warnings`,
  and an image of page 1 filled with a short sample of the user's own profile.
- **Look at the image** and compare it to what the user asked for. Fix and check again until it
  matches. Say briefly what you changed.
- `invalid_template`: the message says what is wrong. Fix exactly that, then check again.
- `pages` over 1: the design is too tall. Tighten the spacing or the font size.
- `busy`: wait a few seconds and retry. `daily_cap_reached`: stop and tell the user.

## 4. Save once the user approves
- Show the user the result and ask before saving. Then call
  `save_template(kind, source=..., name=..., save=true)`.
- `name_taken`: ask whether to replace that template (`replace=true`) or use another name.
- Only pass `make_primary=true` when the user wants HitApply to use it by default.
- Give the user `open_url` to see the full preview in HitApply.

## 5. Use it
- **Résumé:** offer to generate with it: `queue_job(job_id, template=<name>)` for a job, or for an
  existing application have the user regenerate from HitApply.
- **Cover letter:** `queue_job`'s `template` picks résumé templates only. New cover letters use the
  user's primary cover template, so ask before saving whether it should be the one used, and pass
  `make_primary=true` if so (later: save it again with `replace=true, make_primary=true`).
- To write the résumé content yourself with the new template, use the tailor skill's
  `get_materials` → `compose_resume` flow.
