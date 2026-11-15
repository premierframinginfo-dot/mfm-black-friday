# MisterFrameMan Black Friday 2026 landing page

Static page, no build. Pushing to `main` publishes it on GitHub Pages within about a minute.

- `index.html`: the page. Everything that changes with the offer is at the top of its `<script>`: `SCHEDULE`, `CLOSE` and `LINKS`.
- `thanks.html`: where Stripe sends people after paying.
- `images/options/`: the day-by-day pictures sent to Gary.

## Changing the offer

Edit `SCHEDULE` in `index.html`. Each row is when a rate starts (UK time), the extra as a fraction, and optionally a threshold (`min`, with `below` for amounts under it). The page recalculates every card, the any-amount box, the gold line under the heading and the Stripe reference on every button.

Option 1 (VIP 50%, public 30% with 50% over £500, Cyber Monday 50% over £300):
```
var SCHEDULE=[
  {start:"2026-11-23T09:00:00+00:00", rate:0.5},                          // VIP window (send the link only to VIPs)
  {start:"2026-11-26T17:00:00+00:00", rate:0.5, min:500, below:0.3},
  {start:"2026-11-30T00:00:00+00:00", rate:0.5, min:300, below:0.3}
];
```
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

`LINKS` holds four Payment Links (pay £200, £500, £1,000, and "customer chooses the amount"). The links are currently from the Stripe sandbox and take test cards only (4242 4242 4242 4242, any future date, any CVC). Once the live account is verified, create the same four links in live mode and paste them in. Each button adds `client_reference_id=bf26-r<rate>-pay<amount>-get<credit>`, which shows on the payment in Stripe so Gary knows what credit to apply. Every link collects name and email, has an optional "A gift? Tell us who it's for" field, and sends the buyer to `thanks.html`.
