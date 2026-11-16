# MisterFrameMan Black Friday 2026 landing page

Static page, no build. Pushing to `main` publishes it on GitHub Pages within about a minute.

- `index.html`: the page. Everything that changes with the offer is at the top of its `<script>`: `SCHEDULE`, `CLOSE` and `LINKS`.
- `thanks.html`: where Stripe sends people after paying.
- `images/options/`: the day-by-day pictures sent to Gary.
- `calendar/mfm-black-friday.ics`: the "add to calendar" file linked from the heads-up email.

## Changing the offer

Edit `SCHEDULE` in `index.html`. Each row is when a rate starts (UK time), the extra as a fraction, and optionally a threshold (`min`, with `below` for amounts under it). The page recalculates every card, the any-amount box, the gold line under the heading and the Stripe reference on every button.

Option 1 as chosen (VIP 50%, public 30%, Cyber Monday 45% for non-buyers). Add `min:500, rate:0.35, below:0.3` to the middle row if Gary wants a higher rate over £500:
```
var SCHEDULE=[
  {start:"2026-11-23T09:00:00+00:00", rate:0.5, vip:true},                // VIP window: only ?vip=early sees it
  {start:"2026-11-26T17:00:00+00:00", rate:0.3},
  {start:"2026-11-30T00:00:00+00:00", rate:0.45}
];
```
Every email's `<head>` needs `<meta name="color-scheme" content="light only">` and `<meta name="supported-color-schemes" content="light">` so the Gmail app does not flip the dark design (tested 15 Nov 2026).

Option 2 (50% on Black Friday, then 5% a day):
```
var SCHEDULE=[
  {start:"2026-11-26T17:00:00+00:00", rate:0.5},
  {start:"2026-11-28T00:00:00+00:00", rate:0.45},
  {start:"2026-11-29T00:00:00+00:00", rate:0.40},
  {start:"2026-11-30T00:00:00+00:00", rate:0.35}
];
```
Option 3 (50% for everyone, Cyber Monday 50% over £500 only):
```
var SCHEDULE=[
  {start:"2026-11-26T17:00:00+00:00", rate:0.5},
  {start:"2026-11-30T00:00:00+00:00", rate:0.5, min:500, below:0.3}
];
```
Option 4 (VIP 50%, public 30%, Monday 45%):
```
var SCHEDULE=[
  {start:"2026-11-23T09:00:00+00:00", rate:0.5},
  {start:"2026-11-26T17:00:00+00:00", rate:0.3},
  {start:"2026-11-30T00:00:00+00:00", rate:0.45}
];
```
Option 5 (one 50%, Thursday to Monday): the current setting.

`CLOSE` is midnight Monday 30 November in every option.

## Stripe

`LINKS` holds four Payment Links (pay £200, £500, £1,000, and "customer chooses the amount"). The links are LIVE (Premier Framing Stripe account, made 16 Nov 2026) and take real cards. The sandbox versions (test_ links, card 4242 4242 4242 4242) are in the git history if a test run is needed. The custom link has a £50 minimum and £10,000 maximum. Each button adds `client_reference_id=bf26-r<rate>-pay<amount>-get<credit>`, which shows on the payment in Stripe so Gary knows what credit to apply. Every link collects name and email, has an optional "A gift? Tell us who it's for" field, and sends the buyer to `thanks.html`.
