# Money Pit — Daily Budget Tracker

A simple, single-file budget tracker. Log income and expenses, see running totals, and delete entries you don't need. Data is stored in Supabase, so it's shared across any device or browser that loads this page.

## Features

- Add income or expense entries with a date, category, and amount
- Live summary of total income, total expenses, and net balance
- Delete any entry
- No build step, no framework — one HTML file

## Tech stack

- Plain HTML/CSS/JavaScript
- [Supabase](https://supabase.com) (Postgres + REST API) for storage, via the `@supabase/supabase-js` client loaded from a CDN

## Setup

### 1. Create the Supabase table

In your Supabase project's SQL Editor, run:

```sql
create table budget_entries (
  id bigint generated always as identity primary key,
  date date not null,
  type text not null,
  category text not null,
  amount numeric not null
);
```

### 2. Enable Row Level Security and add policies

This app has no login system — it uses Supabase's public `anon` key, so access is controlled entirely through RLS policies. Enable RLS on the table, then add policies for `SELECT`, `INSERT`, and `DELETE` to the `public` role (all with `using (true)` / `with check (true)`).

**Note:** this makes the table publicly readable and writable by anyone who has your Supabase URL, which is visible in the page source once hosted. That's an acceptable tradeoff for a personal tool with no sensitive data, but don't store anything sensitive in it without adding real authentication.

### 3. Add your Supabase credentials

Open `index.html` and set these two constants near the top of the `<script>` block:

```js
const SUPABASE_URL = 'your-project-url';
const SUPABASE_ANON_KEY = 'your-anon-public-key';
```

Both values are found in your Supabase project under **Settings → API**.

### 4. Host it

This is a static file — host it anywhere that serves static HTML. For GitHub Pages:

1. Push `index.html` to your repository
2. Go to **Settings → Pages** in the repo
3. Set the source branch and root folder
4. Your tracker will be live at the generated Pages URL

## Usage

1. Open the page
2. Pick a date (defaults to today), choose Income or Expense, enter a category and amount
3. Click **Add Entry** — it saves to Supabase and appears in the table immediately
4. Click **Delete** next to any row to remove it
5. Totals update automatically

## Notes

- All fields (date, type, category, amount) are required; amount must be a positive number
- Since there's no authentication, treat this as a personal/low-stakes tool rather than a place for sensitive financial data
