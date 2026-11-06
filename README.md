# USC Transplant Surgery Fellowship — Call Schedule

The call-schedule site for the USC Transplant Surgery Fellowship: **https://schedule.usctransplant.life**

This repository holds only the pages. GitHub Pages serves them. They contain no personal data.
The server lives in the private `timeoff-backend` repository, in the `schedule*.gs` files.
Its README covers administration, accounts and handover for all three sites.

## What people can do

**Everyone with a call-schedule role** (View, Edit or Admin):

- View any week or month: services, time off, cross-cover and living donor cases.
- View the year's monthly assignments and each fellow's time off and cross-cover.
- Filter the view to themselves (fellows).

**Fellows with Edit, and admins,** add and remove living donor cases. Fellows can remove cases from today on.

**Admins** use `/admin/`. They can:

- Set the monthly service assignments.
- See who gets each email. People choose their own emails in the portal.
- Turn automatic emails on or off.
- Preview any week's or month's email, and send one-off emails with a message on top.

Assignment rules:

- All four services need a fellow every month.
- Each fellow covers one service per month. Cross-cover doesn't count.
- Liver or HPB may hold two fellows when exactly one is an HPB fellow. The portal marks HPB fellows.
- The HPB fellow stands in for the other fellow on a shared service, so that fellow needs no cross-cover when off.
- HPB fellows never cross-cover any service.

The portal (https://portal.usctransplant.life) manages people, roles and the attendings offered for living donor cases.

## How it fits together

```
Browser ──► this repository (GitHub Pages)
   │  "Sign in with Google" or an emailed code → a sign-in token
   ▼
Apps Script web app (private repository: timeoff-backend, schedule*.gs)
   checks the token, reads and writes the fellowship calendars,
   and sends the weekly and monthly emails as the fellowship account
```

The same server runs the time-off site and the portal. The three sites share one member list and one sign-in.

## Files

| File | Purpose |
|---|---|
| `index.html` | Schedule page for everyone |
| `admin/index.html` | Admin page |
| `assets/schedule.css` | Styles for this site only |
| `assets/api.js`, `assets/ui.js`, `assets/styles.css`, `assets/fonts/` | Shared with `timeoff` and `portal`. Keep them identical |
| `.github/workflows/shared-files.yml` | Fails when the shared files differ from the other two sites |
| `CNAME` | Custom domain for GitHub Pages |

## Making changes

- **Pages**: edit here and commit to `main`. GitHub Pages publishes within a minute or two.
- **Shared files**: make the same change in `timeoff` and `portal`. The **Shared files** check catches differences.
- **Month rules on the admin page**: the server sends them as source text of `scAssignmentErrors_`.
  The page rebuilds the same function, so the rules exist in one place only.
- **Assignment rules, case format, email schedule and wording**: edit `schedule*.gs` in `timeoff-backend`.

Email links open this site at a week or month, on that period's academic year:

- `/?week=2026-11-09&year=2026`
- `/?month=2027-08&year=2027`

From May 1 to July 31, the header switches between this year and next.

## Hosting

- **GitHub Pages**: Settings → Pages → Deploy from branch `main`, folder `/`. Custom domain `schedule.usctransplant.life`. Enforce HTTPS on.
- **DNS** (Squarespace, `usctransplant.life`): `CNAME schedule → usc-transplant-surgery-fellowship.github.io`.
- **Sign-in**: the Google sign-in client lists `https://schedule.usctransplant.life` as an authorized JavaScript origin.
