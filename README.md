# Leaderboard request-order evidence

Local Next.js 16.4.6 app with Chrome and intercepted fixture API responses. All displayed users, tokens, earnings (111/333/999 ZEC), and ranks are synthetic test data, not real accounts or income. No production mutation was performed.

Base: 2ed79c04d4751d1746de62f383aa489bc09d38ff
Fix: 57bf293a6ea3c6b4a8814e77f3515aa8ad840646

Select Last 30 Days, then Last 90 Days. Deliver the 90-day response (999) first and the old 30-day response (333) last. Before: the UI still selects 90 days but shows 333. After: it retains 999.

Four race regressions fail on the baseline and pass with the fix; the latest-request failure control passes both. Additional late-success checks pass at 1280/390 widths in light/dark themes. TypeScript and the full webpack production build pass. The existing build warns about the KPI address-parser WASM async target. APIs/authentication are fixtures; this is not a live backend test.
