# Changelog

All notable changes to the `will` contract are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project uses the `version` field in
[`contracts/will/Cargo.toml`](./contracts/will/Cargo.toml) as its version
identifier. Each entry below the "in Cargo.toml (change grew here)" line
gets its own [contract spec artifact](./spec) once exported.

## [Unreleased]

### Fixed

- `Allocation::FixedAmount` is now denominated in the will's **primary token**
  (`Will::token`, the first entry of the `tokens` list) instead of being paid
  out of *every* token the will holds, and `assert_valid_allocations` validates
  the fixed total against that token's balance rather than the sum across all
  of them. Previously a two-token will with a `FixedAmount(100)` beneficiary
  paid out 100 units of each token while validation compared 100 against the
  combined total, so the two could disagree by a wide margin (#384).
- A `FixedAmount`-only will that leaves value unallocated no longer strands
  that value in the contract. `distribute` refunds the remainder of every token
  to the will's owner and publishes a new `leftover_refunded` event; the will's
  `balances` are cleared and it is marked `Released` as before, but no tokens
  are left with no accounting and no withdrawal path (#383).
- `distribute` finally has its own doc comment. A stack of `///` paragraphs
  describing rounding, multi-token payout and checks-effects-interactions was
  attached to `proportional_share`, leaving `distribute` undocumented and
  `proportional_share`'s rustdoc describing something else, with several
  overlapping or truncated sentences (#385).

### Changed

- `create_will`, `clone_will`, `batch_create_wills`, `split_will`,
  `update_beneficiaries`, `renounce_beneficiary` and `update_will_settings`
  now validate `FixedAmount` commitments against the will's primary-token
  balance. A commitment that only the *sum* of several tokens could cover is
  rejected up front rather than being silently under-paid at release (#384).

### Added

- `leftover_refunded` event (`lftback`), emitted per token when a released
  will returns unallocated value to its owner (#383).

### Removed

- Removed unused `InvalidPercentage` (code 22) error variant from `WillError`.

## [0.1.0] - Initial shipped behavior

Seeded entry summarizing the contract's behavior as of this changelog's
introduction. See the [README's Contract Functions table](./README.md#contract-functions)
and [`spec/will-v0.1.0.json`](./spec/will-v0.1.0.json) for the authoritative,
up-to-date interface.

### Added

- Core will lifecycle: `create_will`, `check_in`, `trigger_will`,
  `emergency_checkin`, `release_inheritance`, `cancel_will`, `close_will`.
- Beneficiary management: `update_beneficiaries`, `renounce_beneficiary`,
  basis-point-based percentage splits (must sum to 10,000).
- Guardian override: up to 3 named guardians, weighted quorum voting via
  `guardian_trigger`, guardian list management via `update_guardians`, and
  a cooldown period after guardian-list changes before a vote can force a
  release.
- Multi-token support: a will can hold balances across multiple SEP-41
  tokens (or native XLM) simultaneously via `top_up`.
- Batch and convenience operations: `batch_check_in`, `batch_create_wills`,
  `clone_will`, `merge_wills`, `set_delegate` (delegated check-in),
  `migrate_will`, `archive_will`.
- Query surface: `get_will`, `get_wills_by_owner`,
  `get_wills_by_owner_and_status`, `get_wills_by_beneficiary`,
  `get_triggered_wills`, `get_protocol_stats`, `get_will_history`,
  `get_contract_version`.
- On-chain audit trail via `WillStatusTransition` records, retrievable
  through `get_will_history`.
- `WillError` numeric error codes for every failure mode (see the
  [README's error code reference](./README.md#error-codes)).
- Resource-cost profiling suite (`docs/RESOURCE_COSTS.md`) and a
  coverage-guided + property-based fuzzing suite (`docs/FUZZING.md`)
  covering `create_will` and `update_beneficiaries`.

[Unreleased]: https://github.com/SoroWill/sorowill-contracts/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/SoroWill/sorowill-contracts/releases/tag/v0.1.0
