## Rust

- Use `bon` when the project already depends on it, or when many optional fields create unreadable `None` runs.
- Use a plain parameter struct when fields are mostly required.
- Use typed-state builders when construction order or required-field presence matters.
- Use named methods for collapsed enum dispatch:
  - `execute_admin_kind()` instead of `execute_kind(ActionKind::Admin)`
- Partial application through a builder is acceptable when repeated setup is real and call sites become clearer.
