# Maintainer notes (Freezyyy-arch)

## #411 reveal_and_claim uses will.balance
Already fixed on `main`: `reveal_and_claim` in `contracts/will/src/lib.rs`
iterates `will.balances` (all locked tokens) and pays the hashed beneficiary's
share of every token, then re-syncs the `will.balance` mirror. No change made.
