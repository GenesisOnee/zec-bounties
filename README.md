# Chat composer regression evidence

Actual local Next.js 16.4.6 UI in Chrome, with intercepted synthetic API responses and fixture authentication. All bounty/user/amount data and the one-pixel PNG are test fixtures, not real work, payments or accounts. No production chat was sent.

Base: 2ed79c04d4751d1746de62f383aa489bc09d38ff
Fix: d262ae59075ee4e905575fb848e0280a16854e37

Before: submit a message, hold its POST response, then edit the still-enabled textarea. The before screenshot shows an unsent replacement draft while Send is pending. Completing the original POST clears that replacement; a browser assertion confirms the expected replacement is lost and the field becomes empty.

After: the textarea, native file input and Remove image button are disabled while sending, matching the existing Send and Attach image controls. A successful acknowledgement clears the submitted draft and re-enables editing. A failed send keeps its text/image, re-enables the controls and allows an edited retry.

Two baseline regression tests fail and the failed-send retention control passes. A separate baseline text-loss reproduction fails on the actual empty value. All three tests pass with the fix, plus four image-send checks at 1280/390px in light/dark. The full webpack production build passes with the repository existing type-validation skip. Standalone tsc reports two identical pre-existing team-page export errors on base and patch (DuePaymentsView and getUserRole); no new type errors. Backend/authentication are fixtures, not a live-backend test.
