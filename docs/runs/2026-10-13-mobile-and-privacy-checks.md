# Run log: mobile layout and student-data checks (2026-10-13)

Run locally on Windows 11 with Playwright Chromium. Status: Verified (re-run 2026-10-13).

## Layout

Method: `python -m http.server 8091`, open each page, compare `document.documentElement.scrollWidth` with `clientWidth`, take a screenshot.

| Page | Width | Horizontal overflow (px) |
|---|---|---|
| index.html | 390 / 1280 | 0 / 0 |
| GNEST305_7Mark_Dashboard.html | 390 / 1280 | 0 / 0 |
| ai_ds_7mark_numerical(1).html | 390 / 1280 | 0 / 0 (was 415 / 0) |
| ai_ds_7mark_numerical(3).html | 390 / 1280 | 0 / 0 |
| ai_ds_question_bank_demo(1).html | 390 / 1280 | 0 / 0 (was 24 / 0) |
| ai_ds_question_bank_demo(4).html | 390 / 1280 | 0 / 0 (was 46 / 0) |
| seminar_report_review.html | 390 / 1280 | 248 / 0 (not changed, see PR) |

`index.html`: 0 page errors and 0 console errors on load at both widths; the "Interactive Labs (8)" tab opens.
Page titles of `ai_ds_7mark_numerical(3).html` and `ai_ds_question_bank_demo(4).html` no longer contain raw `<span>` markup.

## Student-identifying data in HEAD

- `git grep` over HEAD for the student's name and for the college register-number pattern: no matches. (The exact strings are not repeated here.)
- A register-number-shaped pattern (`[0-9]{2}[A-Z]{2}[0-9]{3,}`) and the words "register", "roll no", "reg no" over every `*.html`: no matches.
- `GNEST305_7Mark_Solved.pdf`: 8 pages, no matches for those patterns; author metadata is an institution name, not a person.
- `seminar_report_review.html` no longer names the student or gives a register number, but it still shows the review's grades and a college chip. It is not linked from the portal.
- History: a `git log --all -S` search for the old register number lists 777870f (which added it) and 94819ec (which removed it). The old commits still contain it.
