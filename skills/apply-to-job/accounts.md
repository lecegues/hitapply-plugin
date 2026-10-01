# Sign-in and account pages

Use on any page that asks the user to sign in or create an account before applying.

## Is it set up?
Job-site accounts need Playwright MCP and the user's secrets file, which holds one job-site email
and password as `JOB_EMAIL` and `JOB_PASSWORD`. **You never see or ask for the values.** You type
the literal **name** (`JOB_EMAIL`, `JOB_PASSWORD`) with `browser_type` / `browser_fill_form`;
Playwright fills in the real value, and snapshots show it as `<secret>JOB_EMAIL</secret>`.

Type `JOB_EMAIL` into the email field, then snapshot:
- It shows `<secret>JOB_EMAIL</secret>`: it's set up. Continue below.
- It shows plain `JOB_EMAIL`, or you're using Claude in Chrome: it isn't set up. Clear the field,
  and ask the user to sign in (or create the account) in that tab and say done. Then continue.

## Sign in once
- Fill email `JOB_EMAIL` and password `JOB_PASSWORD`, and press Sign In. **Only once.** Never retry
  a failed sign-in: repeated attempts can lock the account.
- Signed in: go back to where you came from (SKILL.md step 2, or `dock.md`).

## No account yet
If sign-in fails with "no account", "user not found" or similar, or the page only offers to create
one: create the account with email `JOB_EMAIL` and `JOB_PASSWORD` in **every** password field
(including "confirm password"). Tick only the consent boxes the site requires to create an account,
and mention them in the summary.

## Existing account with another password
If sign-in fails with a wrong password, or account creation says the email is already registered,
ask the user:
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
