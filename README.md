# Illumix

Illumix is a DMX lighting controller built entirely in Rust.  The project is
split into three crates that communicate through a shared `fixture_lib` and
allow you to control real fixtures from a web‑based user interface.

<img width="auto" height="100" alt="Illumix Logo" src="https://github.com/user-attachments/assets/194f0473-11ee-4631-b2cf-2e5cc24b1ebf"/>

## Project Structure
- **dmx_backend** – command line application that loads patching information,
  manages the DMX universe and exposes it over a WebSocket interface.
- **egui_frontend** – WebAssembly frontend built with [`egui`](https://www.egui.rs/)
  providing fixture editors, a colour picker and a fader page.
- **fixture_lib** – shared library that defines fixtures, components, the DMX
  universe and helpers to serialize/deserialize patching information.

A more in‑depth description of each crate is available in
[`docs/architecture.md`](docs/architecture.md).

## Prerequisites
- [Rust](https://www.rust-lang.org/) with 2024 edition support (nightly may be
  required).
- `wasm32-unknown-unknown` target for the frontend:
  `rustup target add wasm32-unknown-unknown`.
- [`trunk`](https://trunkrs.dev/) for building and serving the WebAssembly
  frontend: `cargo install trunk`.

## Building and Running
### Backend
```bash
cargo run -p dmx_backend
```
The backend loads `dmx_backend/patching/patching.json`, transmits DMX values to
hardware (serial or Art‑Net) and serves a WebSocket endpoint on
`ws://127.0.0.1:8000`.

### Frontend
```bash
cd egui_frontend
trunk serve --open
```
`trunk` builds the WebAssembly application and serves `index.html` with the UI.
The frontend connects to the backend’s WebSocket and reflects any changes to
fixtures or fader values in real time.

## Patching Files
Fixtures are configured through JSON files. `patching.json` lists all fixtures
in the universe and references fixture preset files (e.g. `1kw.json`).
See [`docs/patching.md`](docs/patching.md) for the full format.

## Project Goals
- [ ] Compete with other lighting software
- [x] Understand the DMX protocol
- [x] Have fun
- [x] Maybe even get a nice result

## Progress
To follow development, visit the [project board](https://github.com/users/justEres/projects/2).

