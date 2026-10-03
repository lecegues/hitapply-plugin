# BrowserOS neo: tool map and differences

Use when the `browseros-neo` tools are connected. Follow SKILL.md, `dock.md` and `form.md` as
written, swapping each Playwright tool for its neo equivalent below. Pass `agentName` on every
call, and the `page` id from `tabs` on every page tool.

| Playwright | BrowserOS neo |
|---|---|
| `browser_tabs` | `tabs` (`list`, `new`) |
| `browser_navigate` | `navigate` (`action: "url"`; also `reload`) |
| `browser_snapshot` | `snapshot` (`mode: "interactive"` on long forms) |
| `browser_click` | `act` `kind: "click"` |
| `browser_mouse_click_xy` | `act` `kind: "click_at"` |
| `browser_fill_form` | `act` `kind: "fill"` with `fields[]` |
| `browser_type` (`slowly`) | `act` `kind: "type"` (focus the field first) |
| `browser_press_key` | `act` `kind: "press"` |
| `browser_select_option` | `act` `kind: "select"` |
| `browser_wait_for` | `wait` `for: "text"` (returns `matched`, check it) |
| `browser_evaluate` | `evaluate` |
| `browser_file_upload` | `upload` on the `<input type="file">` ref |
| `browser_take_screenshot` | `screenshot` |

`act` returns what changed on the page, so you rarely need a snapshot right after it; pass
`diff: "summary"` on big pages. `run` (one JS script) may batch several steps.

## Differences
- **Files:** download to `%TEMP%\hitapply-files\` (macOS: `$TMPDIR/hitapply-files/`); any local
  path uploads. Call `upload` with the absolute path on the file input's ref; no file-chooser
  click is needed. Delete the folder at the end.
- **Sign-in and account pages: don't use `accounts.md`.** neo has no secrets file, so you never
  type a password or the `JOB_EMAIL` / `JOB_PASSWORD` names. Call `request_human_help` with
  `kind: "login"` and a reason like "Sign in to <company>'s Workday". Then `await_human_help` until
  it's resolved and continue where you were. On Workday, `dock.md`'s "not set up" path applies.
- **Verification codes and CAPTCHAs:** `request_human_help` (`kind: "other"` / `"captcha"`), or
  ask in chat.
- **Comboboxes that ignore a click on the option** (Ashby location): `fill` the text, then `press`
  `ArrowDown`, then `press` `Enter`.
- **Never** use neo's history, bookmarks or connected-app tools. Stay on HitApply and the form.
