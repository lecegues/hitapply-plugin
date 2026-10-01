# Sign-in and account pages

Use on any page that asks the user to sign in or create an account before applying.

## Is it set up?
Job-site accounts need Playwright MCP and the user's secrets file, which holds one job-site email
and password as `JOB_EMAIL` and `JOB_PASSWORD`. **You never see or ask for the values.** You type
the literal **name** (`JOB_EMAIL`, `JOB_PASSWORD`) with `browser_type` / `browser_fill_form`;
Playwright fills in the real value, and snapshots show it as `<secret>JOB_EMAIL</secret>`. A
missing or empty secret is typed as its plain name, so check **both** before using either. Check
on a blank page, **never in a job site's field**: a site's page can read and send whatever is
typed into it, including the real password.
1. Open a **new blank tab** (`about:blank`; Chrome blocks `data:` URLs here), and add two fields
   with `browser_evaluate`:
   `() => { document.body.innerHTML = '<input aria-label="probe-a"><input aria-label="probe-b">'; }`.
   It has no site scripts and no network.
2. Take one snapshot for the refs, then type `JOB_EMAIL` into probe-a and `JOB_PASSWORD` into probe-b.
3. Check with one `browser_evaluate`, which only answers yes/no and clears the fields. Don't
   snapshot the probe after typing (you never need to read a secret back):
   `() => { const [a, b] = document.querySelectorAll('input'); const r = { email: a.value !== '' && a.value !== 'JOB_EMAIL', password: b.value !== '' && b.value !== 'JOB_PASSWORD' }; a.value = b.value = ''; return r; }`
   Both must be `true`.
4. Close the probe tab and go back to the form's tab. Once per run is enough.

If either is `false`, the probe page won't open, or you're using Claude in Chrome, it
isn't set up. Ask the user to sign in (or create the account) in the form's tab and say done. Then
continue. Even when it is set up, type `JOB_EMAIL` only into email fields and `JOB_PASSWORD` only
into password fields.

## Screenshots
Snapshots hide the secrets; **screenshots don't.** On sign-in, account and password-reset pages,
use only `browser_snapshot`. Never take a screenshot there, and never click "show password".

## Sign in once
- Fill email `JOB_EMAIL` and password `JOB_PASSWORD`, and press Sign In. **Only once.** Never retry
  a failed sign-in: repeated attempts can lock the account.
- Signed in: go back to where you came from (SKILL.md step 2, `dock.md` or `form.md`).

## Sign-in failed, or there's no sign-in to try
Sites often say only "Invalid email or password", which doesn't tell you whether the account
exists. Whatever the error, go to account creation: create the account with email `JOB_EMAIL` and
`JOB_PASSWORD` in **every** password field (including "confirm password"). Tick only the consent
boxes the site requires to create an account, and mention them in the summary.

## Email already registered
If account creation says the email is already registered, ask the user:
"You already have a <company> account. Reset its password to your job-site password? (yes/no)"
- **Yes:** click "Forgot password", enter `JOB_EMAIL`, and submit. Ask the user to paste the reset
  link or code from their email. Open the link (or type the code), set the new password with
  `JOB_PASSWORD` in every password field, then sign in once.
- **No:** ask them to sign in themselves in that tab and say done.

## Codes and checks
- **Verification or MFA code:** ask the user in chat, and type exactly what they send.
- **CAPTCHA**, or a sign-in through Google/LinkedIn/SSO: hand back. Ask the user to complete it and
  say done.
- A sign-in that fails after one reset: stop and hand back. Don't loop.

## Never
- Type, print, guess or ask for the real email or password values. Only type the names.
- Use a different email or password than the secrets.
- Read the user's inbox for codes or links. They paste them in chat.
