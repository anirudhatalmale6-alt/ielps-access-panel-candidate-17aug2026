# IELPS Access Panel & Learner Visual Consolidation — frozen candidate, 17 August 2026

Answers the *Access Panel & Learner Visual Consolidation Instruction, 17 August 2026*.

**Nothing here is deployed.** `/learner/` on eilps.com is still the earlier
application. This is the frozen candidate and its evidence, returned for the
Keyword visual sign-off and the DEPLOY APPROVED decision, exactly as section 12
asks.

---

## 1. Release identity

    Repository          10495109/v0-ielps-platform-h4
    Branch              visual-direction-20260815
    Commit              5266b36a018c066472390d7161134353da1daf62
    Commit timestamp    2026-08-17T20:28:36+00:00
    Build ID            DhWcD3chXNKavs2dQSVGs
    Source inventory    source-sha256-inventory.txt  (81 files)
    Inventory hash      5e4b480667325c9aab28e35287b5ed4e39d042f0d1b1936dfeef5ab4485dbffb
    Build command       npm run build   ->  exit 0
    TypeScript          npx tsc --noEmit ->  exit 0, no errors
    Lint                npm run lint    ->  exit 0, clean
    Intended target     /learner/ on eilps.com (port 4302, eilps-learner.service)
    Rollback reference  ~/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery
                        — the currently live tree, untouched by this candidate

### The three commits in the candidate

    5266b36  Keyword pop-up and parent alerts: say what is not yet authored
    578632b  Revert the typography standardisation; it is held from this release
    b48e2f6  Level band and level pages: Continue with {LEVEL}; remove the
             Access Panel's generic placement controls

17 files changed, 211 insertions, 296 deletions.

---

## 2. Scope conformance, section by section

| Section | State |
|---|---|
| 2.1 Approved 16 August Access Panel | Included. The rejected "Choose your access route" modal is not in this tree, and is now gone from live behaviour on the Public Landing as well. |
| 2.2 Six CEFR level pages | Included: A1, A2, B1, B2, C1, C2. |
| 2.3 Conditional level band | `/` shows no band; `?level=A1…C2` renders the band from real curriculum data. Photographed against the live server. |
| 2.4 Final level-band copy | Exactly one **Continue with {LEVEL}**. "Choose a pathway instead" is gone. |
| 2.5 Placement-control correction | Preview placement, the generic Placement navigation and the generic diagnostic section are out of Access Panel Home. The word "placement" survives only inside two descriptive sentences about the adult route — no control, no navigation item, no section. |
| 2.6 Pathway-first routing | Every level CTA resolves to `/?level={LEVEL}#pathways` — the pathway list, never the Adult Lesson Player. |
| 2.7 Reading expansion window | Included. Whole passage preserved, questions alongside, existing submission contract untouched. |
| 2.8 Writing expansion window | Included. Prompt, model line, large writing area, existing activity contract. |
| 2.9 PiP stylesheet restoration | Included. Fixed bottom-right, closed and open both photographed. |
| 2.10 Parent "Needs attention" | Derived from `GET /api/school/parent/dashboard` only. See section 4 below — one invented line was still present and has been removed. |
| 3 Keyword pop-up | Function as approved: word, meaning, contextual example, image where one exists, pronunciation. **Final visual approval is yours to give.** |
| 4 Keyword truthfulness | Strengthened. See section 4 below. |
| 5 Watermarked iStock comp | Not in this candidate. The clean approved Access Panel image is retained. |
| 6 Typography | Hanken Grotesk is out. Plus Jakarta Sans retained on the Access Panel and level pages as the approved 16 August package supplied it. Curriculum typography untouched. |
| 7 Public Landing | Not touched by this candidate at all — different application, different repository. Regression evidence below. |
| 8 Unrelated work | None bundled. No typography standardisation, no trial-entitlement work, no migrations, no vocabulary authoring, no image or audio generation backend, no Stripe QA, no webhook change, no new Studio routes, no signed-in-app rebranding. |

---

## 3. Functional acceptance — measured, not asserted

Run against the built candidate with the live IELPS API attached.

    /                            band CTAs: 0                         PASS (no ?level= -> no band)
    /?level=A1                   band CTAs: 1  "Continue with A1"     PASS
    /?level=B2                   band CTAs: 1  "Continue with B2"     PASS
    /?level=C1                   band CTAs: 1  "Continue with C1"     PASS
    "Choose a pathway instead"   present: False on all three          PASS
    /levels/a1/  h1 "Beginner"           -> /?level=A1#pathways       PASS
    /levels/b1/  h1 "Intermediate"       -> /?level=B1#pathways       PASS
    /levels/b2/  h1 "Upper Intermediate" -> /?level=B2#pathways       PASS
    /levels/c1/  h1 "Advanced"           -> /?level=C1#pathways       PASS
    /levels/c2/  h1 "Proficiency"        -> /?level=C2#pathways       PASS
    /levels/zz/                  HTTP 404                             PASS
    Build / TypeScript / Lint    exit 0 / exit 0 / clean              PASS
    API calls observed           /api/auth/me, /api/auth/pathways,
                                 /api/auth/refresh,
                                 /api/curriculum/deep-catalog         canonical
    /api/eilps dependency        none                                 PASS

No level CTA enters the Adult Lesson Player: every one of them lands on the
pathway list, so the pathway is still chosen once, by the visitor.

---

## 4. Truthfulness — two things I changed, and why

**The keyword pop-up.** It already refused to show a decorative stand-in
picture, and says plainly when no picture exists for a word. It did not say
anything about the rest. So a learner could reasonably have read a runtime
example sentence and a browser-synthesised voice as reviewed IELPS content.
For any word that carries no approval or version metadata from the permanent
2,976-entry programme, the window now says so: no approved picture yet, the
meaning and example come from the lesson runtime, and the pronunciation is the
device's voice rather than a supplied recording. The notice disappears on its
own the moment a word is authored and approved — nothing to undo later.
`60_keyword_platform.jpg` (no picture) and `60_keyword_book.jpg` (a real
picture for that exact word).

**The parent "Needs attention" panel.** Its live rows were already derived only
from server values. Its *sample* rows were not: one of them read "Max:
listening dipped — suggest a listening booster", which is a performance
threshold no server value supports. Replaced with the same kind of statement
the transform can actually produce. `22_…live_data.jpg` shows the panel filled
from a real dashboard payload with the **Live** badge; `23_…empty_state.jpg`
shows it saying "Nothing needs your attention right now" rather than filling
itself.

---

## 5. The Public Landing has not moved

Exact, not statistical: **no file in the live landing build has changed since
the 12:43 UTC deployment.**

    ~/eilps/frontend/dist          directory timestamp  17 Aug 12:43
    files modified since 13:00     0
    build manifest sourceSha256    6f98fced717047ba4c957d4deca3f719d65285f2e68a541489b8cb9535b4a21e

`40_landing_now_*.jpg` are the live landing captured again this evening. Against
this morning's post-deployment evidence the mean per-pixel difference is
1.5/255 with about 100 pixels of a million exceeding a visible threshold, which
is JPEG re-encoding noise between two compressed captures, not a visual change.
The zero changed files above is the stronger statement of the two.

---

## 6. What I could not prove here, stated plainly

- **The keyword pop-up's final look is yours to approve, not mine to assume.**
  Function approval is not visual approval; that distinction is the reason
  these screenshots exist.
- **Signed-in states.** The parent dashboard was photographed by supplying the
  exact payload shape `/api/school/parent/dashboard` returns. That proves the
  derivation and both states; it is not a live signed-in session.
- **The level pages name levels differently from the specification you sent
  today.** See section 8.

---

## 7. Deployment is not a file swap — it needs the Administrator

The live learner service serves a static export:

    WorkingDirectory  ~/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery
    DIST              …/out
    ExecStart         /usr/bin/node …/runtime/learner-server.mjs

This candidate is a Next.js **standalone** application — it has dynamic routes
(`/app/[slug]/placement`, `/app/[slug]/onboarding`) that a static export cannot
produce, so it cannot be dropped into `out/`. Deploying it means a new release
directory plus a change of `eilps-learner.service` to run the standalone server
on port 4302. That is a systemd change and therefore Administrator work; I have
no sudo and will not attempt it. On approval I will hand over the exact release
directory, the exact unit file diff and the exact rollback command — the current
release directory stays untouched, so rollback is pointing the unit back at it
and restarting.

Expected impact: the learner service restarts once. Seconds, not minutes. The
API, the database, the worker and the Public Landing are not involved.

---

## 8. Two questions before this ships

**1. Level names.** The approved 16 August package names the levels

    A1 Beginner · A2 Elementary · B1 Intermediate ·
    B2 Upper Intermediate · C1 Advanced · C2 Proficiency

The Account Flow and Pathways Specification you sent today gives

    B2 Inter Plus · C1 Upper-Intermediate · C2 Advanced

The top three rungs are one step apart between the two documents, and your own
specification says conflicting labels must be referred before publishing. There
is also a third name in play: the live curriculum server calls A1 "Foundation
English", which is what the level band shows. I have changed nothing. Tell me
which naming is canonical and I will make all three agree in one pass.

**2. The example routes in section 5 of the account-flow specification.**
`/register?pathway=…`, `/login/student-code`, `/placement`, `/checkout`,
`/school`, `/authoring` do not exist as addresses on the platform today. The
same journeys do exist under different addresses — the registration handoff
deployed this morning is `/?pathway=<id>` and it already accepts exactly your
seven account ids. If section 5 is describing intent, it is satisfied. If those
literal URLs are the contract, that is a routing change to the live application
and needs its own approval.

---

## 9. Files

    50_panel_home_desktop_live.jpg      Access Panel Home, desktop
    51_panel_home_pathways_live.jpg     the seven account types
    52_panel_home_tablet_live.jpg       tablet
    53_panel_home_mobile_live.jpg       mobile
    54_level_band_A1_live.jpg           level band, real curriculum data
    54_level_band_B2_live.jpg
    54_level_band_C1_live.jpg
    55_level_band_A1_mobile_live.jpg    level band, mobile
    56_access_ladder_live.jpg           the CEFR ladder page
    57_access_ladder_mobile_live.jpg
    58_level_page_a1_live.jpg           the six level pages
    58_level_page_b1_live.jpg
    58_level_page_b2_live.jpg
    58_level_page_c1_live.jpg
    58_level_page_c2_live.jpg
    60_keyword_book.jpg                 keyword pop-up, word with a picture
    60_keyword_listen.jpg               a verb
    60_keyword_platform.jpg             a word with no picture yet
    61_reading_expanded.jpg             reading expansion window
    62_writing_expanded.jpg             writing expansion window
    63_pip_closed.jpg                   PiP closed
    64_pip_open.jpg                     PiP open
    22_parent_needs_attention_live_data.jpg
    23_parent_needs_attention_empty_state.jpg
    40_landing_now_*.jpg                the live Public Landing this evening
    source-sha256-inventory.txt         81 source files, SHA-256 each
