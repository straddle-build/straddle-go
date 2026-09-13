# Changelog

## [1.0.4](https://github.com/straddle-build/straddle-go/compare/v0.1.0...v1.0.4) (2026-09-13)


### ⚠ BREAKING CHANGES

* **api:** 16 breaking changes to the SDK surface.
    - Response content type of `bridge.createBankAccountPaykey` changed from `text/plain` to `application/json`.
    - `400` error response of `bridge.createBankAccountPaykey` changed from `error_response` to `error_response`.
    - Response content type of `customers.create` changed from `text/plain` to `application/json`.
    - `400` error response of `customers.create` changed from `error_response` to `error_response`.
    - Response content type of `charges.create` changed from `text/plain` to `application/json`.
    - `400` error response of `charges.create` changed from `error_response` to `error_response`.
    - Response content type of `payouts.create` changed from `text/plain` to `application/json`.
    - `400` error response of `payouts.create` changed from `error_response` to `error_response`.
    - Property `payout.created_at` is now required.
    - Property `payout.created_at` type changed from `string<date-time> | null` to `string<date-time>`.
    - Property `payout.updated_at` is now required.
    - Property `payout.updated_at` type changed from `string<date-time> | null` to `string<date-time>`.
    - Property `unmasked_payout.created_at` is now required.
    - Property `unmasked_payout.created_at` type changed from `string<date-time> | null` to `string<date-time>`.
    - Property `unmasked_payout.updated_at` is now required.
    - Property `unmasked_payout.updated_at` type changed from `string<date-time> | null` to `string<date-time>`.
* **api:** 4 breaking changes to the SDK surface.
    - Property `embed_error_response.data` type changed from `unknown | null` to `unknown`.
    - Schema `customer_address` shape changed.
    - Schema `unmasked_compliance_profile` shape changed.
    - Schema `compliance_profile` shape changed.

### Features

* **api:** initial SDK generation ([4494e2b](https://github.com/straddle-build/straddle-go/commit/4494e2b7e44285ddb492fc4e2870f9042be2248d))
* **api:** update property embed_error_response.data (+3 more changes) ([295355c](https://github.com/straddle-build/straddle-go/commit/295355ca85ce02dc5e83cb3dc778e2fd5f995769))
* **api:** update SDK surface (17 changes) ([5000d6b](https://github.com/straddle-build/straddle-go/commit/5000d6b0bb023be074521ae5beb20e035c24a898))


### Chores

* release 1.0.4 ([f4655d8](https://github.com/straddle-build/straddle-go/commit/f4655d85f97b5a0628c0c3111afca242c3d247fe))
* release 1.0.4 ([ed58846](https://github.com/straddle-build/straddle-go/commit/ed588468bb65bea2588f2970c49f9f930140332e))
