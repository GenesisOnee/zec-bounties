# Pending team-favorite request regression

Base 2ed79c04d4751d1746de62f383aa489bc09d38ff; patch 8e8c73de8f4fb3bf3ab1fc01a3bf53cde5eea893. Actual local Next.js UI in installed Chrome, with synthetic teams, account, authentication and API responses. No production data, backend or authentication was exercised.

Reproduction: open /explore, click Alpha’s star, hold its POST before the fixture saves it, then click the same star again. On unchanged main the DELETE finishes first. Releasing the first POST saves Alpha, but the UI shows zero favorites and an unselected star. The failing test checks both server-fixture state and the visible button. The team detail page also permits a second click while its request is pending.

The fix admits one request per team at a time in the context, and disables that team’s star/button with aria-busy until the request settles. Optimistic updates remain immediate; failed requests roll back and can be retried. A different team can still save independently.

Four tests fail on unchanged main and two controls pass. All six pass after the fix: directory/detail pending controls, pending removal, delayed-POST consistency, failed-save rollback and retry, and concurrent saves for different teams. Eight additional checks pass across both pages in light/dark themes at 1280/390px. Desktop light and mobile dark screenshots were visually inspected.

Production next build --webpack passes under the existing repository configuration. Standalone tsc --noEmit exactly matches the saved same-base baseline: two existing invalid team-page exports, DuePaymentsView/getUserRole (separately addressed by PR616). The repository already skips type validation in next build; this patch does not change that setting.

Run: yarn playwright test --config playwright.browser.config.ts --project team-favorites. The local launcher used installed Chrome and is not part of the patch.
