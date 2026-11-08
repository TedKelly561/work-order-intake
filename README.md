# Work Order Intake

Paste a messy work-order email and get a clean, ready-to-dispatch service ticket in seconds.

**▶ [Live demo: tedkelly.dev/projects/work-order-intake](https://tedkelly.dev/projects/work-order-intake/)** · part of the [Service Business Toolkit](https://tedkelly.dev/projects/)

![Work Order Intake screenshot](docs/screenshot.png)

**Try it:** https://tedkelly.dev/projects/work-order-intake/

## The problem

Work orders reach a service business as emails, and no two look alike. A warranty company sends a formatted dispatch. A homeowner sends "help!! water everywhere" with the address buried in the middle. A vendor forwards a request and says the claim number will follow.

Someone in the office then retypes each one into the ticket system. That takes about five minutes per order. Details get missed along the way: the alternate phone number, the elderly resident, the $100 service fee the warranty company already collected. Warranty companies also grade you on how fast you make first contact, so a slow handoff costs you.

## What it does

- **Reads the email** and fills in the ticket: customer, phone numbers, service address and ZIP, claim number, warranty company, trade (HVAC, plumbing or appliance), equipment, a one-line problem summary and urgency.
- **Shows how sure it is.** Each field is marked *found*, *inferred* or *needs review*, so the office checks only what it needs to.
- **Flags what a person needs to act on:** no claim number yet, a vulnerable occupant, water or gas possibly shut off, a fee already collected, an old unit that may need a replacement quote, a referral to thank.
- **Starts the first-call clock** using each warranty company's deadline (24 or 48 hours, 4 hours for direct customers, 1 hour for emergencies).
- **Suggests a technician:** the one in the right trade with the fewest jobs booked today and tomorrow.
- **Drafts the first text to the customer**, ready to copy and send.
- **Adds the ticket to today's intake queue**, with live countdowns and a "Mark called" button.

Every field can be edited before the ticket is saved.

## How it works

Paste → Review → Dispatch.

There are two extraction engines:

1. **Built-in parser (default).** Plain JavaScript rules that run in your browser, with no key, no server and no cost per email. They read labelled fields ("Claim #:", "Property:"), recognize each warranty company's claim number format and sender domain, normalize phone numbers to `(555) 013-5521`, score keywords to choose the trade, and look for urgency cues such as "water everywhere", "no heat" or "elderly".
2. **Claude (optional).** Open "Use Claude for extraction", paste your own Anthropic API key and tick the box. The email is sent straight from your browser to `api.anthropic.com` (model `claude-sonnet-5`), which returns strict JSON with the same fields. The key stays in the browser tab (memory, or session storage if you choose) and is never sent anywhere else. If the call fails for any reason, the tool falls back to the built-in parser and says so.

For a real business this would connect to the inbox (or a forwarding address) and the ticket system, so tickets appear in the queue without anyone pasting anything.

## For developers

- `index.html`: page markup
- `app.css`: page styles (shared tokens come from `shared/demo.css`)
- `app.js`: the parser (`window.Intake`, pure functions you can test in Node) plus the UI
- `shared/data.js`: the fictional company, techs, vendors, jobs and sample emails (`window.SUMMIT`)

Quick parser test in Node:

```js
global.window = global;
require('shared/data.js');
const Intake = require('./app.js');
const e = SUMMIT.sampleEmails[1];
console.log(Intake.parse(`From: ${e.from}\nSubject: ${e.subject}\n\n${e.body}`, SUMMIT));
```

The queue is stored in `localStorage` (wrapped in try/catch, so it still works in a private window, just without saving).

## Built with

Plain HTML, CSS and JavaScript, with no framework and no build step. It is hosted on GitHub Pages. Claude via the Anthropic Messages API is optional.

## Note

Summit Home Services and its warranty partners are fictional. Every name, address, phone number and claim number in this demo is made up.

## Run it locally

No install or build step. Clone the repo and open `index.html` in a browser, or serve the folder:

```bash
git clone https://github.com/TedKelly561/work-order-intake.git
cd work-order-intake
python3 -m http.server 8000   # then open http://localhost:8000
```

`shared/data.js` holds the fictional company (Summit Home Services) shared by every demo in the toolkit.

## More from the toolkit

- [Work Order Intake](https://github.com/TedKelly561/work-order-intake): messy email → clean service ticket
- [Quote Builder](https://github.com/TedKelly561/quote-builder): price-book quotes with margin view and approve links
- [Dispatch Board](https://github.com/TedKelly561/dispatch-board): drag-and-drop dispatch with first-call deadline alerts
- [Owner's KPI Dashboard](https://github.com/TedKelly561/kpi-dashboard): the numbers that run a service business

## About

Built by **Ted Kelly**, who runs a service business and builds the software behind it. Want something like this for your business? Visit **[tedkelly.dev](https://tedkelly.dev)**.

## License

[MIT](LICENSE). All companies, people, addresses and claim numbers in this demo are fictional.
