# Form path: fill the form yourself

Use when the HitApply dock doesn't appear on the form (Lever, Ashby, SmartRecruiters, a company's
own careers page...). You fill each page from the profile, attach the documents and stop before
the final Submit.

## Get the files
- Call `get_download_links(application_id)`. The links expire in 10 minutes, so download right away.
  On `not_available`, ask the user to attach the files themselves and continue.
- Download each non-null link with the shell into `hitapply-files/` **inside your current working
  directory** (Playwright MCP can only upload files from there). Name them after the user, since
  recruiters see the file name, e.g.:
  `curl -sfL -o "hitapply-files/<First>_<Last>_Resume.pdf" "<resume url>"`, and `..._Cover_Letter.pdf`
  for the cover.
- If `cover` is null, there's no cover letter. Leave optional cover fields empty and say so in the summary.

## Each page
1. Take **one** snapshot of the page (`browser_snapshot` / `read_page`).
2. Fill everything you can map from the profile in **one** call (`browser_fill_form` /
   `form_input`): name, email, phone, location, links (LinkedIn, GitHub, portfolio), work and
   education history, and current company/title. Copy values exactly as the profile has them.
3. Upload the résumé, then the cover letter if there's a field for it, one at a time, using the
   absolute path of the downloaded file:
   - Playwright MCP: click that field's upload control (Attach / Upload / Choose file) first, so
     its file chooser opens, then call `browser_file_upload` with that one path.
   - Claude in Chrome: `file_upload` with the field's ref and the path.
   If a site parses the résumé and overwrites fields, re-check them against the profile.
4. **Free-text questions** ("Why this company?"): draft 2–4 sentences from the profile and the job
   description, and note that you drafted them.
5. **Ask the user in chat** for salary, visa or sponsorship, relocation, start date, legal or
   background questions and EEO/demographics, unless the profile states the answer outright.
   Ask all of a page's questions in one message.
6. Leave optional fields you can't answer empty. Don't invent anything.
7. Click **Next** / **Continue**, then repeat from 1. On the page with the final **Submit**, stop
   and go to step 4 of SKILL.md.

Snapshot again only after something changes the page (a new page, a section that expands). Don't
snapshot after every field.

A sign-in or create-account page that appears along the way: follow `accounts.md`, then continue
with the form where you left off.

## Stop and hand back
A CAPTCHA, an error you can't fix after one retry, or a
required field you have no answer for after asking.
