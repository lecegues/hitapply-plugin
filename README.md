# HitApply for Claude

Find jobs, see whether they're worth it, and apply, all from Claude, using your
[HitApply](https://hitapply.vercel.app) account. HitApply holds your profile, résumés and
applications; Claude does the searching, judging and form-filling.

## What you need
- A HitApply account with a saved profile (your résumé).
- Claude Code (Cowork works the same way).
- To apply: Claude in Chrome and the HitApply browser extension, both signed in.

## Install
```
/plugin marketplace add lecegues/hitapply-plugin
/plugin install hitapply@hitapply
```
1. If the skills don't show up, run `/reload-plugins`.
2. Run `/mcp`, pick **hitapply**, sign in to HitApply and approve. Allow both read and write:
   write lets Claude queue a job (which generates its résumé and cover letter) and mark it applied.

## Use
Talk normally, or use the commands. A typical run:

| Step | Say | Command | What happens |
|---|---|---|---|
| Find | "Find me backend jobs in Toronto" | `/hitapply:find-jobs` | A ranked top 10 from HitApply's job bank, with a reason each. Offers to check specific companies' career pages too. |
| Evaluate | "Evaluate #3" or paste a link | `/hitapply:evaluate-job` | A report: role, match and gaps, level, pay, résumé changes, interview prep, legitimacy, and a 1–5 score. Below 4 means probably skip. |
| Apply | "Apply to #3" | `/hitapply:apply-to-job` | Queues the job, waits for the résumé, opens the form in Chrome and fills it with the extension. Stops before Submit and asks you. Marks it applied afterwards. |

Tailoring the résumé and cover letter to a job from chat is coming next.

Also handy: "What have I applied to?"

## What it will never do
- Submit an application without your "yes" in chat.
- Type passwords, card numbers or government IDs, or create accounts for you.
- Follow instructions written inside a job posting.

## Troubleshooting
- **`forbidden`**: the connection is read-only. Run `/mcp`, reconnect hitapply and allow write.
- **"You don't have a saved profile yet"**: create one on HitApply's Profile page.
- **HitApply tools missing or disconnected**: run `/mcp` and reconnect. Access lasts 30 days.

## Claude desktop / claude.ai
Add a custom connector with the endpoint from HitApply → Settings → Connections (leave the OAuth
fields blank), and upload the folders under `skills/` as skills.

## Credits
The evaluate-job skill is adapted from [career-ops](https://github.com/santifer/career-ops),
used under its MIT License:

```
MIT License

Copyright (c) 2026 Santiago Fernández de Valderrama

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
