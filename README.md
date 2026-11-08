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

Every signed-in team member sees the same list. Changes appear for other people automatically (Supabase Realtime), and the page also re-checks every minute and when you come back to the tab.

## Sign-in and team members

People sign in with a **username and password** (Supabase Auth). A username like `priya` is stored in Supabase as `priya@actiontracker.local`; nobody needs to type the email part.

- `public.profiles` links each Auth user to the team: `username`, `display_name`, `is_admin`, `must_change_password`.
- Only users with a profile can read or change actions (row level security), so someone who self-registers through the Supabase API sees nothing.
- New accounts get a **starting password** and must choose their own the first time they sign in.
- Admins see a **Team** link: add a member with a starting password, reset someone's password, or remove a member. This runs through the `admin-users` Edge Function, which checks that the caller is an admin before using the service role key. The service role key never reaches the browser.
- `actions.created_by` records who added each action.

Starting accounts: `priya` (admin), `dan`, `marcus`.

To make someone an admin, run in the Supabase SQL editor:

```sql
update public.profiles set is_admin = true where username = 'dan';
```

Recommended in the Supabase dashboard:
- Authentication → Sign In / Providers: turn off **Allow new users to sign up** (accounts are created by admins).
- Authentication → Attack Protection: turn on **leaked password protection** if your plan includes it.

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
