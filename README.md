# HitApply for Claude

Find jobs, see whether they're worth it, and apply, all from Claude, using your
[HitApply](https://hitapply.vercel.app) account. HitApply holds your profile, résumés and
applications; Claude does the searching, judging and form-filling.

## What you need
- A HitApply account. No profile yet? The onboard step builds one from your résumé.
- Claude Code (Cowork works the same way) or Codex.
- To apply: a dedicated Chrome/Edge profile with the HitApply extension and the Playwright MCP
  Bridge extension. Setup:
  [AUTO_APPLY_SETUP.md](https://github.com/lecegues/HitApply/blob/main/docs/agent/AUTO_APPLY_SETUP.md).

## Install
```
/plugin marketplace add lecegues/hitapply-plugin
/plugin install hitapply@hitapply
```
1. If the skills don't show up, run `/reload-plugins`.
2. Run `/mcp`, pick **hitapply**, sign in to HitApply and approve. Allow both read and write:
   write lets Claude save your profile, queue a job (which generates its résumé and cover letter), apply the edits you
   approve, and mark it applied.

**Codex:**
```
codex plugin marketplace add lecegues/hitapply-plugin
codex plugin add hitapply@hitapply
codex mcp login hitapply --scopes hitapply:read,hitapply:write,hitapply:answers
```

## Use
Talk normally, or use the commands. A typical run:

| Step | Say | Command | What happens |
|---|---|---|---|
| Onboard | "Set up my profile" and attach your résumé | `/hitapply:onboard` | Reads your résumé, shows what it found, and saves it as a new HitApply profile once you approve. Then asks a few questions to fill gaps. |
| Find | "Find me backend jobs in Toronto" | `/hitapply:find-jobs` | A ranked top 10 from HitApply's job bank, with a reason each. Offers to check specific companies' career pages too. |
| Evaluate | "Evaluate #3" or paste a link | `/hitapply:evaluate-job` | A report: role, match and gaps, level, pay, résumé changes, interview prep, legitimacy, and a 1–5 score. Below 4 means probably skip. |
| Tailor | "Tailor my résumé for #3" | `/hitapply:tailor` | Proposes rewordings of your résumé (then cover letter) aimed at the job, before → after. Applies only what you approve. HitApply blocks changed numbers and some invented skills, then rebuilds the PDF. |
| Apply | "Apply to #3" | `/hitapply:apply-to-job` | Queues the job, waits for the résumé, opens the form in your applying profile and fills it (with the extension's dock where it appears, itself elsewhere). Stops before Submit and asks you. Marks it applied afterwards. |

Also handy: "What have I applied to?"

## What it will never do
- Submit an application without your "yes" in chat.
- Type card numbers, government IDs or your passwords. It creates or signs in to job-site accounts
  only if you set up the optional job-site secrets file, and it never sees that password.
- Follow instructions written inside a job posting.

## Troubleshooting
- **`forbidden`**: the connection is read-only. Run `/mcp`, reconnect hitapply and allow write.
- **"You don't have a saved profile yet"**: say "set up my profile" and attach your résumé, or create one on HitApply's Profile page.
- **HitApply tools missing or disconnected**: run `/mcp` and reconnect. Access lasts 30 days.

## Claude desktop / claude.ai
Add a custom connector with the endpoint from HitApply → Settings → Connections (leave the OAuth
fields blank), and upload the folders under `skills/` as skills. Applying also needs Playwright MCP
(see the setup guide).

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
