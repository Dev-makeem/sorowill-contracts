# Maintainer notes (Valreb001)

## #421: hashed beneficiary commitments in events

Already addressed on main. `add_hashed_beneficiary` emits only the on-chain
`commitment`, which is `SHA-256(address_bytes || salt)` over a 64-byte
pre-image (32 bytes of address plus a 32-byte random salt chosen by the
beneficiary; see `reveal_and_claim`). No user-chosen password is hashed, so the
public event cannot be used for an offline dictionary attack.
