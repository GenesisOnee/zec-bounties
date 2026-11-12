# Staff profile pagination checks

Base 2ed79c04d4751d1746de62f383aa489bc09d38ff; patch a8c239f2fcf05aa9391c256b40bc8929849f8fa2. Actual local Next.js UI in Chrome, using synthetic API, user and authentication fixtures. All displayed accounts, jobs and rewards are test data. No production account, authentication, permissions or payout was changed.

Open /users/staff-fixture as the fixture admin and expand Staff view. The fixture follows the actual endpoint contract: each request returns both lists, with 100 rows per page and independent next offsets. It has 201 open and 201 history rows.

Before: load more open (200 rows), then load more history. Open grows to 300 rows although its offset did not change; React logs duplicate row keys. Reversing the loading order duplicates history instead. After: both lists retain exactly 201 unique rows in original order after every page, and refreshed rows replace their previous values.

Three browser regressions fail on unchanged main; five pass after the fix, including the existing chain-reset and ordinary-hunter behavior. Four additional light/dark 1280/390px checks pass. Desktop light and mobile dark screenshots were visually inspected. The full production webpack build passes. Standalone TypeScript diagnostics exactly match the saved same-base baseline: two existing invalid team-route exports (DuePaymentsView/getUserRole, separately addressed by PR616). No live backend, database or production-authentication test claim.

Run: yarn playwright test --config playwright.browser.config.ts --project staff-profile. Local runs used installed Chrome through an uncommitted launcher.
