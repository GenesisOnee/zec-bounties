# Bounty dialog state regression evidence

Actual local Next.js UI in Chrome; API and authentication use the repository's intercepted synthetic fixtures. Displayed names, accounts, bounties, amounts and deliverable URLs are test data. No production submission was sent.

Base: 2ed79c04d4751d1746de62f383aa489bc09d38ff
Fix: 855e8cd60cde48dc832e2e9c5849d2b59c9778aa

Before: edit the submitted work for bounty one, close the dialog and open bounty two. The second bounty inherits the first draft and link in its edit form. Separately, hold the response to a submission for bounty one, close it, open bounty two and type a draft. Completing the first request replaces the second form with the first bounty's submitted work. Validation messages also survive switching. Both /home and /my-bounties reproduce all three failures.

After: each different bounty ID has a separate component instance. The second bounty starts with its own data and late callbacks cannot replace its form. Reopening the same bounty still preserves an existing submission edit. Six regressions fail on baseline; two same-bounty controls pass. All eight pass after the fix, as do four viewport/theme variants and four existing remote-status/submission-broadcast cases. Screenshots show the late-response case at1280/390px in light/dark; before screenshots also show the misplaced edit.

Full webpack production build passes with unchanged ignoreBuildErrors config. Standalone TypeScript reports exactly the same two existing team-page export errors on baseline and patch (DuePaymentsView and getUserRole), separately addressed in PR616. Full Node/backend suites were not rerun for this dialog-only change.
