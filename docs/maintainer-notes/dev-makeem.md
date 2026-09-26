# Maintainer notes (Dev-makeem)

## #412 get_will_history unbounded response

Not duplicated here. Open PR #407 ("Bound WillHistory and add a paged history read") bounds the stored history in storage.rs and adds a paged read in lib.rs, with tests in get_will_history_test.rs and documentation of the maximum response size. Main still returns the raw vector until #407 merges.

