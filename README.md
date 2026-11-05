# Fellowship Call Schedule

The call-schedule site for the USC Transplant Surgery Fellowship: **https://schedule.usctransplant.life**

- **Everyone** (fellows on the approved list who are in the Google Group, and admins) can see any week or month —
  the same sections as the weekly/monthly emails: services, time off, cross-cover and living donor cases — filter it to
  just themselves, see the year's monthly assignments, and **add or remove living donor cases**.
- **Admins** (`/admin/`) change the monthly service assignments, the attendings list, who gets the emails (weekly,
  monthly, previews), turn automatic emails on or off, and send one-off emails with a message at the top.

## How it fits together

```
Browser ──► this repository (GitHub Pages: the pages people see)
              │  "Sign in with Google" → a Google ID token
              ▼
            Google Apps Script web app (private repository: timeoff-backend, files schedule*.gs)
              checks the token, then reads and writes the fellowship Google Calendars and sends the emails
              as the fellowship account
```

The same server runs the time-off site (https://timeoff.usctransplant.life). Both sites share the people list
(People & access on the time-off admin page) and the sign-in. This repository holds only the pages — no personal data.

| File | What it is |
|---|---|
| `index.html` | Schedule page (everyone) |
| `admin/index.html` | Admin page |
| `assets/schedule.css` | Styles used only by this site |
| `assets/site.js` | Which server to use. Empty = the live server named in `assets/api.js` |
| `assets/styles.css`, `assets/api.js`, `assets/fonts/` | **Shared with the time-off site** — keep identical in both repositories (a GitHub check compares them) |
| `CNAME` | The custom domain for GitHub Pages |

## Making changes

- Page changes: edit here and commit to `main`; GitHub Pages publishes within a minute or two.
- Shared files (`styles.css`, `api.js`, fonts): make the same change in the `timeoff` repository. The **Shared files**
  check on the Actions tab warns when the two copies differ.
- Rules (assignment rules, living donor case format, email schedule and wording) are in the private `timeoff-backend`
  repository (`schedule*.gs`), deployed by GitHub Actions.

Links in the emails open this site at a week or month: `/?week=2026-11-09` or `/?month=2026-12`.

## Hosting

GitHub Pages: Settings → Pages → Deploy from branch `main` / root, custom domain `schedule.usctransplant.life`, Enforce HTTPS.
DNS (Squarespace, domain usctransplant.life): CNAME `schedule` → `usc-transplant-surgery-fellowship.github.io`.
Sign-in: the Google Cloud OAuth client lists `https://schedule.usctransplant.life` as an authorized JavaScript origin.
