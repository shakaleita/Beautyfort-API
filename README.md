# BeautyFort API — Legacy Integration Review

> **Status: legacy / integration disabled pending security rewrite. Do not deploy or activate as-is.**

This repository contains historic BeautyFort SOAP and WordPress/WooCommerce integration experiments. The existing plugin source identifies **Joe Carré** as its author, and some included examples and dependencies identify additional third-party authors. Retain original licensing and attribution when reviewing or adapting the code. Ownership of these original components must not be represented as the portfolio owner's original authorship.

## Security status

A former public browser-side admin script included a credential-like value and client-side SOAP authentication logic. The exposed string has been removed from the current JavaScript, but **removing it from a current file does not remove it from Git history, existing clones, or caches**. If it was a real credential, the corresponding provider credentials **must be revoked or rotated immediately**.

The legacy stock lookup has deliberately been **disabled** both in the browser JavaScript and in the WordPress AJAX PHP handler, rather than leaving an insecure integration apparently working. No third-party stock API integration was tested against production services.

Read [SECURITY_REVIEW.md](SECURITY_REVIEW.md) for the findings and a safe migration checklist.

## Repository contents

- `Plugin/BeautyFort.php` — historic WordPress admin/plugin integration (disabled)
- `Plugin/assets/js/admin.js` — legacy browser-side code with stock-action disabled
- `nusoap-0/` — third-party NuSOAP library
- `example.php`, `index.html` and `js/` — earlier integration/demo experiments

## Before re-enabling

A modern replacement should use server-side secret storage and HTTPS, explicit request authorisation and nonce checks, strict validation, safe output escaping, provider API timeouts, tested error handling, compatible WordPress/WooCommerce APIs, and integration tests with mocked SOAP responses. Do not copy the old client-side signing workflow into a new system.

## Portfolio context

This is a **legacy code-security and migration case study** rather than a finished original application. See the other projects on [the Digital Solution9 portfolio](https://github.com/shakaleita/teamforge-saas/blob/main/PORTFOLIO.md).
