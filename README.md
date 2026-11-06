# Fellowship Call Schedule

The call-schedule site for the USC Transplant Surgery Fellowship: **https://schedule.usctransplant.life**

- **Everyone with a role on the call schedule** (set in the portal: viewer, fellow or admin) can see any week or month —
  the same sections as the weekly/monthly emails: services, time off, cross-cover and living donor cases — see the
  year's monthly assignments and each fellow's time off and cross-cover. Fellows can filter it to just themselves.
- **Fellows and admins** can also **add or remove living donor cases** (fellows: from today on).
- **Admins** (`/admin/`) change the monthly service assignments (one fellow per service; Liver and HPB can also have an HPB
  fellow, ticked in the portal; the HPB fellow doesn't count as cross-cover for the other fellow), who gets the emails (weekly,
  monthly, previews), turn automatic emails on or off, and send one-off emails with a message at the top.

People, roles and the attendings offered for living donor cases are managed in the portal, https://portal.usctransplant.life.

## How it fits together

```
Browser ──► this repository (GitHub Pages: the pages people see)
              │  "Sign in with Google" → a Google ID token
              ▼
            Google Apps Script web app (private repository: timeoff-backend, files schedule*.gs)
              checks the token, then reads and writes the fellowship Google Calendars and sends the emails
              as the fellowship account
```

The same server runs the time-off site (https://timeoff.usctransplant.life) and the portal (https://portal.usctransplant.life).
The three sites share the member list (the portal) and one sign-in. This repository holds only the pages — no personal data.

| File | What it is |
|---|---|
| `index.html` | Schedule page (everyone) |
| `admin/index.html` | Admin page |
| `assets/schedule.css` | Styles used only by this site |
| `assets/styles.css`, `assets/ui.js`, `assets/api.js`, `assets/fonts/` | **Shared with the time-off site and the portal** — keep identical in all three repositories (a GitHub check compares them) |
| `CNAME` | The custom domain for GitHub Pages |

## Making changes

- Page changes: edit here and commit to `main`; GitHub Pages publishes within a minute or two.
- Shared files (`styles.css`, `ui.js`, `api.js`, fonts): make the same change in the `timeoff` and `portal` repositories.
  The **Shared files** check on the Actions tab fails when the copies differ.
- The month rules on the admin page (all four services covered, one service per fellow, Liver/HPB pairs) come from the
  server as the same function it checks every save with, so they're only written once (`scAssignmentErrors_`).
- Rules (assignment rules, living donor case format, email schedule and wording) are in the private `timeoff-backend`
  repository (`schedule*.gs`), deployed by GitHub Actions.

Links in the emails open this site at a week or month, on that week's academic year: `/?week=2026-11-09&year=2026`
or `/?month=2027-08&year=2027`. From May 1 to July 31 the header switches between this year and next.

## Hosting

GitHub Pages: Settings → Pages → Deploy from branch `main` / root, custom domain `schedule.usctransplant.life`, Enforce HTTPS.
DNS (Squarespace, domain usctransplant.life): CNAME `schedule` → `usc-transplant-surgery-fellowship.github.io`.
Sign-in: the Google Cloud OAuth client lists `https://schedule.usctransplant.life` as an authorized JavaScript origin.
