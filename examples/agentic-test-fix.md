# Example: fix one failing test

Raw request: "write a prompt so Cursor fixes the flaky checkout test."

Target tool was named, so the package can be written. Paths below are the ones the user would confirm before pasting.

```text
## 1. Prompt
You are fixing one failing test in this repo. MUST change only
src/app/checkout/checkout-session.test.ts. NEVER edit the implementation,
the test helper, config, or any other file. NEVER add a dependency.
NEVER weaken, skip, or delete the assertion to make it pass.

Start state: `npm test -- checkout-session` fails on the session-expiry
assertion. Target state: that command passes, and the assertion still
checks that an expired session redirects to /checkout/expired.

Copy the arrange/act style from src/app/billing/billing-session.test.ts.
Leave the production code alone. If the failure is in the implementation,
stop and report the file and line.

Done when: the command above exits 0, the expiry assertion is still
present, and git diff shows only the test file.

Only change what was requested. Do not add features, files, or refactors.

Proof required:
- Full output of `npm test -- checkout-session`.
- `git diff --stat` showing only src/app/checkout/checkout-session.test.ts.
- The expiry assertion quoted from the file after the edit.

Stop-and-ask:
- The fix appears to require an edit outside that test file.
- A new dependency, schema change, or secret would be required.
- The failure message does not match the expiry assertion.

## 2. Context pack
Always-on:
- AGENTS.md (commands and test runner). Trusted.
- .cursor/rules (if present). Trusted.

Spec slice:
- Checkout spec, section "Expired session": an expired checkout session
  redirects to /checkout/expired. Do not load the rest of the spec.

Files to load:
- src/app/checkout/checkout-session.test.ts (editable, trusted).
- src/app/billing/billing-session.test.ts (pattern only, trusted, read-only).
- src/lib/checkout/session.ts (read-only, trusted). Load only if the test
  failure cannot be understood from the test file.

Live evidence:
- Failing assertion name and message only. Omit the full test log.

Handoff / do not reload:
- Earlier retries that mocked the clock in three different styles. The
  conclusion is already known: use the fake timer from the billing test.
  Do not reload those attempts.

## 3. Constraints
Locked:
- The expiry assertion text and the redirect path.
- src/lib/test/clock.ts. Read-only.

Editable:
- src/app/checkout/checkout-session.test.ts

Append-only:
- A short keep/discard note in the reply. Do not rewrite prior notes.

Human approval:
- Any new dependency, schema change, credential, deploy, or merge.

Proof required:
- Test command output, diff stat, and the assertion quote.

Stop-and-ask:
- Scope change, missing fixture, or a failure outside the named assertion.

Budgets:
- 5 edit attempts or 20 minutes. On crash or timeout, stop and log the
  last command. Do not retry forever.

Enforce outside the prompt:
- Cursor deny rules for every path except the test file.
- CI runs the same test command. The agent does not edit the CI config.

## 4. Eval loop
Deterministic:
- `npm test -- checkout-session` exits 0.
- Diff is limited to the one test file.
- The string "/checkout/expired" is still in the assertion.

Rubric:
- Correctness 0.5: the expiry case passes for the right reason.
- Scope 0.3: no other file changed.
- Evidence 0.2: proof artifacts are in the reply.
- Passing bar 0.9 because checkout is on the money path.
- A failed critical dimension (scope or the assertion removed) fails the run.

Keep / discard:
- Keep a diff that passes the deterministic checks.
- Roll back a diff that passes by deleting or loosening the assertion.

Stop when:
- Deterministic checks pass, or the attempt/time budget is hit, or a
  human gate is reached.

Log:
- Append one line per attempt: command, pass or fail, files touched.

Target: Cursor
Why: The task is a single-file edit with a binary test, which matches an agent that can run the test and show a diff.
```

Review scope, forbidden actions, and stop conditions before pasting. Confirm paths match the real project.
