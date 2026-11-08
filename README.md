# Meeting Action Tracker

A single-page app for keeping track of the actions agreed in meetings.

- Add an action with an owner and a due date
- Actions are grouped by owner, each with a tick box for done
- Open, done and overdue counts at the top, with overdue actions shown in red
- Filter by All / Open / Overdue / Done, copy open actions as text for minutes or email, clear done actions, undo deletes

## How data is stored

Actions are stored in Supabase (project `zzqkkecgnrlgxyevwfbx`), in the `public.actions` table:

| column | type | notes |
|---|---|---|
| id | uuid | primary key |
| text | text | the action, 1–500 characters |
| owner | text | 1–80 characters |
| due | date | |
| done | boolean | |
| done_at | timestamptz | when it was ticked |
| is_example | boolean | sample rows, removable with "Remove examples" |
| created_at | timestamptz | |

Everyone who opens the app sees the same list. Changes appear for other people automatically (Supabase Realtime), and the page also re-checks every minute and when you come back to the tab.

**Access:** the app has no sign-in. The table's row level security policies let anyone holding the publishable key read, add, edit and delete actions, and that key is visible in the page source. So anyone with the URL has full access. Don't put confidential information in actions. To restrict access, add Supabase Auth and tighten the policies to `authenticated`.

## Project structure

```
index.html   The whole app: HTML, CSS and JavaScript in one file
vercel.json  Vercel settings (clean URLs, basic security headers, no stale caching of the page)
```

There is no build step. `supabase-js` (pinned to 2.50.0) loads from jsDelivr and fonts load from Google Fonts.

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
