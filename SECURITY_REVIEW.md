# Security review — BeautyFort legacy WordPress integration

## Immediate action

**Treat previously committed client-side credentials as exposed.** The public JavaScript contained a credential-like value embedded in its source. Current code no longer includes it, but it may remain available through Git commit history, forks, mirrors and caches. Never assume deleting a string from the latest commit revokes it. **Rotate or revoke it at the relevant service provider if it was real.** Rewriting a public repository's history is a disruptive action requiring a separate, carefully approved plan; none was performed.

## Current mitigations

- Replaced credential-like values in `Plugin/assets/js/admin.js` with empty values.
- Prevented the browser's stock-lookup submission.
- Disabled the PHP AJAX SOAP execution path with a failing 503 response; access control also rejects users without `manage_options` permission.
- Removed the saved-secret value from the PHP admin form.
- Preserved original authorship and license statements.

These are **containment measures**, not evidence the historical integration is safe to deploy.

## Security and compatibility gaps to resolve

1. **Server-side credentials:** use WordPress/server environment or secret manager; never ship secrets in JavaScript, HTML, source control or logs.
2. **Modern SOAP/HTTPS:** the old integration uses a plain HTTP WSDL URL. Require a trustworthy HTTPS endpoint with certificate validation and explicit timeouts before any network integration.
3. **WordPress authorisation:** check capabilities and action nonces; validate each input according to expected schema, including stock codes and stock quantities.
4. **Safe output:** escape admin form values with appropriate WordPress escape functions; avoid printing sensitive values or raw third-party responses.
5. **WooCommerce behaviour:** mock provider API and test inventory changes, concurrency, failures, idempotency and compatibility with current product stock APIs.
6. **Library provenance:** the historic NuSOAP/other third-party files need license, maintenance, vulnerability and ownership review before reuse.
7. **Tests:** write repeatable WordPress/PHP unit/integration tests and run on supported WordPress/PHP versions.
8. **Disclosure:** do not present upstream authored code as original Digital Solution9 work.

## Scope of this change

The legacy feature has been intentionally disabled on the repository's default branch. It was not deleted. It was not run against a real provider, and no Git history, forks or external accounts were modified. This document is a technical audit, not a formal penetration test or certification.
