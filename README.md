# Meeting Action Tracker

A single-page app for keeping track of the actions agreed in meetings.

- Add an action with an owner and a due date
- Actions are grouped by owner, each with a tick box for done
- Open, done and overdue counts at the top, with overdue actions shown in red
- Filter by All / Open / Overdue / Done, copy open actions as text for minutes or email, clear done actions, undo deletes

## How data is stored

Everything is saved in the browser's `localStorage` under the key `meeting-action-tracker-v1`.
That means:

- Data stays on the device and browser that entered it. Other people opening the same URL see their own, separate list.
- Clearing browser data, or using a private/incognito window, removes the saved actions.

## Project structure

```
index.html   The whole app: HTML, CSS and JavaScript in one file
vercel.json  Vercel settings (clean URLs, basic security headers, no stale caching of the page)
```

There is no build step and there are no dependencies. Fonts load from Google Fonts.

## Deploy to Vercel

1. Sign in at [vercel.com](https://vercel.com) with your GitHub account.
2. Click **Add New → Project** and import this repository.
3. Leave the settings as they are:
   - Framework Preset: **Other**
   - Build Command: leave empty
   - Output Directory: leave empty (the repository root is served)
4. Click **Deploy**.

Every push to `main` redeploys the site automatically.

## Run locally

Open `index.html` in a browser, or serve the folder:

```
npx serve .
```
