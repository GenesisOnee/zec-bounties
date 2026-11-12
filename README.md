# Complete team filter results

Base 2ed79c04d4751d1746de62f383aa489bc09d38ff; fix 1024961d172e2f125c95fcf192ae80e8ea25998a. Actual local Next.js UI in Chrome, with synthetic intercepted API/authentication fixtures. Names, accounts, jobs and displayed rewards are test data; no production account or bounty was changed.

Open /home?team=team-a&q=needle with51teamjobs and the only matching title on page2. Before:0matches, no pagination control, and page2 never requested. After:1match and the expected51stjob displayed. Highest-reward sorting similarly considers the complete team. A page2failure displays Retry; retry succeeds while preserving filters. Switching teams while the old team's page2fails leaves the new team's correct result. Without a filter, ordinary50-row pagination remains.

Three regressions fail on unmodified main. All12board browser tests pass after the fix, including five new team tests and seven existing board controls. Four additional1280/390light/dark search cases pass. Full webpack build passes with unchanged ignoreBuildErrors; tsc diagnostics exactly match the saved same-base baseline's two invalid team-page exports, separately addressed by PR616. No backend, live database or production-auth validation is claimed. The screenshots were visually checked.
