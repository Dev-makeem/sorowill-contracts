# Maintainer notes (rdj-savyy)

## Issue #416: archive_will leaves guardian votes and history behind

Open PR #408 ("Drop history and guardian vote entries when archiving a will") already removes the `WillHistory` entry and every `GuardianVote` / `GuardianCancelVote` entry in `archive_will`, documents that history does not survive archival, and adds tests in `archive_will_test.rs`. On main, `archive_will` already removes the will from the owner, beneficiary and Triggered indexes. No duplicate code is added here; merging #408 resolves this issue.
