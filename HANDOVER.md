# Your Live Dashboard — Owner's Guide

_Toowong French Patisserie · built July 2026_

---

## What this is

A private web page that shows your key numbers, updating on their own from your
own software. It lives at:

**https://french-patisserie-dashboard.cgravestein.workers.dev**

Open it in any browser, type your password, and you're in. It works on your
phone too.

---

## How to log in

1. Go to the address above.
2. Type the password you set during setup.
3. That's it. It remembers you on that device for 30 days.

If you ever forget the password, tell your AI — it can help you reset it.

---

## Your cards — and where each number comes from

Every dollar figure comes from **Xero**, always **ex-GST**, matching your P&L
to the cent. The only thing that comes from your till is the count of sales.

| Card | What it means | Where it comes from |
|---|---|---|
| **Revenue** | All your trading income (sales), ex-GST | Xero |
| **Transactions** | How many completed sales you rang up | Lightspeed (once connected) |
| **Average Customer Spend** | Revenue ÷ number of sales | Xero ÷ Lightspeed |
| **Cost of goods** | Everything in your Cost of Sales accounts | Xero |
| **Wage %** | Wages + super ÷ Revenue | Xero |
| **Overheads** | Operating expenses, minus the wages inside them | Xero |
| **Profit** | Revenue − Cost of goods − Wages − Overheads | Xero |

**A note on Wage %:** your kitchen wages sit inside Cost of goods in your books,
and your front-of-house/admin wages sit in Operating Expenses. To keep Profit
exactly right, the Wage % card tracks your front-of-house/admin labour; the
kitchen labour is already counted inside Cost of goods. This was your choice at
setup, and it means Profit reconciles to your Xero P&L to the cent.

---

## What's connected

- ✅ **Xero (accounting)** — live. All money figures. Reconciled to your June
  P&L to the cent.
- ⏳ **Lightspeed (till / sales count)** — built and ready, waiting on Lightspeed
  to switch on API access for your account (you emailed developers@kounta.com).
  When they reply and enable it: create an Integration in your Lightspeed Back
  Office, and your AI will paste the two codes in — a two-minute job. Until then,
  the Transactions and Average Customer Spend cards show "not configured" (never
  a fake number).
- ➖ **Urhere (rostering)** — nothing to connect. Urhere doesn't offer a live
  link for other tools to read from; it sends your timesheets straight into Xero
  instead. That means your **Wage % is already covered by Xero**. The only extra
  Urhere could have added is a *projected* Wage % (a forecast from your draft
  roster) — that card shows "not configured", which is the correct, honest state.

---

## Your settings (you can change these any time)

On the **Settings** screen inside the dashboard:

- **Timezone:** Brisbane
- **Trading day:** rolls over at midnight (standard)
- **Wage % target:** 38% — the Wage % card flags amber if you go above this
- You can also set targets for Cost of goods, Overheads and Profit, change the
  venue name, week start, default period, and accent colour.

Nothing here touches any code. Click, and it saves.

---

## When the Lightspeed email comes back

1. Lightspeed enables API access → the **Create a new Integration** button
   appears in your Back Office (under Sites → Integrations, developer area).
2. Create one named "Dashboard", set its Redirect address to:
   `https://french-patisserie-dashboard.cgravestein.workers.dev/auth/pos/callback`
3. Save it — it shows a **Client ID** and a **Client Secret**.
4. Paste both to your AI. It walks you through adding them safely (they go into
   Cloudflare's locked secret store, never into a file or an email).
5. Open the dashboard's **Connections** screen and click **Connect** for the
   till. Choose your company, grant access — done. Average Customer Spend goes
   live.

---

## If something looks off

- A card says **"not configured"** — that source isn't connected yet. That's
  honest, not broken.
- A connection says **needs reconnecting** — open Connections and click
  Reconnect.
- Numbers look wrong — check the date range at the top first; then tell your AI.

---

## If ALL the numbers go blank ("no data yet" on every card)

This usually means your **Xero app secret has expired** (Xero expires these
periodically for security — expect it roughly once a year). The connection can
still *say* "Connected" while this happens. Fix it like this:

1. On the dashboard, click **Refresh** first — if that doesn't fix it, continue.
2. Go to **https://developer.xero.com/app/manage** → open **French Patisserie
   Dashboard** → **Configuration**.
3. Under **Client secrets**, click **Generate a secret**. Copy the new secret
   (shown once only).
4. Go to **https://dash.cloudflare.com** → Workers & Pages →
   **french-patisserie-dashboard** → **Settings** → **Variables and secrets**.
5. Add/edit a **Secret** named `ACCOUNTING_CLIENT_SECRET`, paste the new value,
   and click **Deploy**.
6. Back on the dashboard: **Connections** → **Reconnect** next to Xero → log in
   and allow access to Toowong French Patisserie.
7. Numbers come straight back. (Or just paste the new secret to your AI and it
   will walk you through steps 4–6.)

The registered reconnect address, for reference, is:
`https://french-patisserie-dashboard.cgravestein.workers.dev/auth/accounting/callback`

Everything is read-only: the dashboard can only *look at* your Xero and till,
never change anything.
