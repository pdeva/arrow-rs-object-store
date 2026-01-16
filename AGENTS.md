# Repository Guidelines

## Project Structure & Module Organization
The crate source lives in `src/`, with backend implementations in `src/aws/`, `src/azure/`, `src/gcp/`, and `src/http/`. Local and in-memory stores are in `src/local.rs` and `src/memory.rs`, while shared utilities (paths, clients, config) are in modules like `src/path/`, `src/client/`, and `src/config.rs`. Integration-style tests are in `tests/` (for example, `tests/http.rs` and `tests/get_range_file.rs`). Release and process notes live under `dev/`, and `Cargo.toml` defines features and MSRV.

## Build, Test, and Development Commands
- `cargo build` — build with default features (`fs`).
- `cargo build --features aws` (or `azure`, `gcp`, `http`) — compile a specific backend.
- `cargo build -p object_store --target wasm32-unknown-unknown` — wasm build (cloud backends are not supported on this target).
- `cargo test` — run unit and feature-gated tests.
- `TEST_INTEGRATION=1 cargo test --features aws` — run integration tests after configuring the backend emulator and env vars.

## Coding Style & Naming Conventions
The project targets Rust edition 2024 with MSRV 1.85. Use `rustfmt` defaults; keep modules and functions in `snake_case`, types in `CamelCase`, and constants in `SCREAMING_SNAKE_CASE`. Feature-gate backend-specific code with `#[cfg(feature = "...")]`.

## Testing Guidelines
Run the full unit suite with `cargo test`. Integration tests require `TEST_INTEGRATION=1` plus backend-specific configuration. See `CONTRIBUTING.md` for Localstack (AWS), Azurite (Azure), and Fake GCS Server (GCP) setup steps and required environment variables. Prefer adding tests alongside the module they exercise, or in `tests/` for cross-cutting behavior.

## Commit & Pull Request Guidelines
Use short, imperative commit subjects; recent history commonly uses Conventional Commit-style prefixes like `feat:`, `fix:`, or `build(deps):`, sometimes with scopes. PRs should describe the change, link relevant issues, and list the tests run (including any integration emulators used).

## API Deprecation
Deprecations should use `#[deprecated(since = "...", note = "...")]` with the next release version and a clear migration hint. Deprecated APIs typically remain for at least two major releases before removal.
