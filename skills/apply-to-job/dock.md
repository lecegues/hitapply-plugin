# Dock path: run the extension's Auto mode

Use when the HitApply dock shows on the form.

## Sign-in and account pages (Workday)
- The dock fills the email and ticks the consent boxes. It presses Create Account by itself once
  the password is in; Sign In you (or the user) press.
- **Job-site secrets set up** (see "Is it set up?" in `accounts.md`): type `JOB_EMAIL` into the
  email field (the dock's email may differ), and `JOB_PASSWORD` into every password field, by ref.
  On a Sign In page, press Sign In once. Then follow `accounts.md` for a failed sign-in, an
  existing account or a reset.
- **Not set up:** you never touch the password. Tell the user: "Click the password box in the
  <company> Workday tab and pick Chrome's suggested (or saved) password. If it's a Sign In page,
  press Sign In. Then say done." Wait for their reply.
- If the dock says **Verify your email**, ask the user for the code (or link) from Workday's email
  in chat and type it in. Then continue.
- If the dock says **Already applied** or **Job closed**, stop and tell the user.
- A CAPTCHA: hand back to the user.

## Run Auto mode
- The HitApply dock sits at the bottom right. If it's minimized, click it to open it.
  It lives in an open shadow root, and the snapshot lists its buttons: click them by ref. Don't click the page's own "Autofill my application" button (Greenhouse's).
- If the dock shows **"Already in HitApply"**, click **"Back to autofill"**.
- **Multi-page forms (Workday):** Auto presses Save and Continue itself. Never click Next,
  Continue or Save and Continue yourself. Just wait for the dock to stop.
- **Check the dock's résumé line before anything else.** It must read this application's
  company — title. The dock prefers the last application picked in the extension (for up to 12 h)
  over this page, so it can show a different job. If it shows another job, click **SWITCH**. If it
  says "No résumé loaded", click **CHOOSE**. Then pick this application. Don't start Auto until
  the line matches, or the wrong documents get attached.
- Tick **"Auto — all steps, stop before submit"**, then click **"Auto-fill every step"**.
- Wait with `browser_wait_for` for the text "Application ready" (30 s per call; repeat up to ~10
  times). If it hasn't appeared, take one snapshot to check for a question card. It stops in one of
  two states:
  - **"Application ready"**: check you are really on the last step (Review, or a page with the
    final Submit). If not (Auto can stop early), tell the user which step and which required
    fields are empty, then follow their answer. Otherwise go to step 4 of SKILL.md.
  - **A question card** ("Let AI answer this" / "Skip this"): the dock couldn't fill a field.
    - Click **"Let AI answer this"** first (at most once per field). It usually takes a few seconds.
      Don't pick an option yourself while the dock is answering, even if you know the answer.
    - Only if the field isn't filled within ~30 s, or an error toast appears, fill it yourself
      (from the résumé, `answers` or the job) and click **Resume**.
    - For short free-text questions ("Why this company?"), draft 2–4 sentences from the
      profile and the job description, and note that you drafted them.
    - For **salary, visa or sponsorship, relocation, start date, legal or background
      questions, and EEO/demographics**, use `answers` if it states the answer; otherwise ask the
      user in chat.
- **Never clear or re-enter fields the dock already filled.**

## When the dock path fails
Switch to filling it yourself if any of these happens:
- a question card you can't clear (Let AI answer + your own answer + Resume didn't work);
- no progress for ~3 minutes;
- a dock error or "HitApply didn't respond";
- "Application ready" on a step that isn't the last, and you can't fill what's left by hand.

Then:
1. **Turn Auto off first:** untick "Auto — all steps, stop before submit" in the dock (by ref) and
   check in a snapshot that it's unchecked. Otherwise Auto starts again after the reload. Never use
   "Hide HitApply on this site".
2. Reload the form tab (`browser_navigate` to its URL), and follow `form.md` from the page you're
   on. Keep answers already saved on the site (Workday keeps earlier steps); don't redo them. Tell the
user in one line that the dock stalled and you're filling it yourself.

