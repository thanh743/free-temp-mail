# Free Temp Mail on Your Own Domain

A short, practical guide to using disposable inboxes for sign-ups, OTP codes and QA testing — and when to move from a shared temp address to a catch-all inbox on a domain you own.

Hosted service used in the examples: **[Free Domain Mail temp mail](https://freedomainmail.com/)** — free disposable inbox in one click, plus free catch-all inboxes on your own domains.

## Why use a temporary inbox?

- **One-time codes and magic links** — receive a verification email without exposing your real address.
- **QA and end-to-end tests** — every test run can use a fresh address, so sign-up flows never collide with old accounts.
- **Newsletters and downloads** — read the one email you need, then forget the address.

## Shared temp domain vs. your own domain

| | Shared temp address | Catch-all on your own domain |
|---|---|---|
| Setup | None, one click | Add MX/TXT records once |
| Who can read it | Anyone who knows the address | Only you (private inbox) |
| Blocked by sign-up forms | Often (known disposable domains) | Rarely — it is your domain |
| Retention | Minutes | Much longer |
| Good for | Throwaway codes | Testing, per-service aliases, long-lived projects |

Rule of thumb: **never use a public temp inbox for an account you care about.** Anyone who guesses the address can read the mail, including password-reset links.

## Using a catch-all domain

With a catch-all inbox, *any* address at your domain is delivered — you do not create aliases in advance:

```
shop-a@yourdomain.com
newsletter-b@yourdomain.com
qa-run-1742@yourdomain.com
```

Benefits:

1. **Per-service addresses** — if one address starts receiving spam, you know exactly who leaked it and can drop that single address.
2. **Unlimited test identities** — generate `qa-<timestamp>@yourdomain.com` in your test suite.
3. **No typo bounces** — mail sent to a misspelled name still arrives.

## DNS checklist (one time)

1. Point the domain's **MX** records to your mail provider.
2. Add the **TXT** verification record the provider gives you.
3. Add **SPF** (and **DKIM** if you plan to send) so other servers trust your mail.
4. Optionally add a **DMARC** policy, starting with `p=none` while you monitor reports.
5. Send a test message to a random address at the domain and confirm it arrives.

## Example: fresh address per test run

```js
// Generate a unique address for every end-to-end test
const domain = process.env.TEST_MAIL_DOMAIN; // e.g. yourdomain.com
const address = `qa-${Date.now()}@${domain}`;
// 1. sign up with `address`
// 2. poll your inbox (or the provider's API) for the verification email
// 3. extract the OTP code / magic link and continue the flow
```

## Related

- [email-dns-checklist](https://github.com/thanh743/email-dns-checklist) — SPF, DKIM, DMARC and MX checklists
- [temp-mail-api-examples](https://github.com/thanh743/temp-mail-api-examples) — receive OTP codes and magic links in tests

## License

MIT
