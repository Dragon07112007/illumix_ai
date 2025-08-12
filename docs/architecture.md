# Illumix Architecture

Illumix is organized as a Rust [workspace](../Cargo.toml) containing three crates that
work together to control DMX lighting equipment.

## Crates

### `fixture_lib`
This library defines the core data structures shared by the project:

- [`Fixture`](../fixture_lib/src/fixture.rs) – a lighting fixture with an ID, name and starting DMX address.
- [`FixtureComponent`](../fixture_lib/src/fixture.rs) –
  components such as colour, dimmer, position and custom values that
  generate channel data for a fixture.
- [`Universe`](../fixture_lib/src/universe.rs) – a collection of fixtures that
  can export or import its DMX channel values as JSON.
- [`Patching`](../fixture_lib/src/patching.rs) – loads fixture presets and
  constructs a [`Universe`](../fixture_lib/src/universe.rs) from a user supplied
  patching file.

The library is used by both the backend and the frontend to share a common
representation of fixtures and DMX state.

### `dmx_backend`
The backend is a command‑line application that manages the DMX universe and
communicates with hardware or networked controllers.

- Loads the patching description and builds a [`Universe`](../fixture_lib/src/universe.rs).
- Sends channel data to real hardware via a serial interface or over
  the network using Art‑Net (`dmx.rs`).
- Exposes the current [`Universe`](../fixture_lib/src/universe.rs) over a
  WebSocket server so that the frontend can modify it in real time (`server.rs`).

### `egui_frontend`
A WebAssembly application built with [`egui`](https://www.egui.rs/) that
provides an interactive user interface.  It connects to the backend via
WebSocket and allows users to:

- View and edit fixtures and their components.
- Pick colours and synchronise them to selected fixtures.
- Control intensities with a bank of virtual faders.

## Data Flow
1. The backend loads `patching/patching.json` and resolves fixture presets
   into a `Universe`.
2. DMX values are periodically transmitted to hardware or broadcast via
   Art‑Net.
3. The backend hosts a WebSocket server that exchanges the serialized
   `Universe` with the frontend.
4. The frontend deserializes the `Universe`, provides editing widgets and
   sends updates back to the backend.

## Patching Files
Fixture definitions and patching information live in JSON files.
`patching.json` describes which fixtures are used and where they appear in
DMX space. Each fixture references a fixture preset file (for example
`1kw.json`) that specifies the fixture name and its component list.
Further details on the file format are available in
[`docs/patching.md`](patching.md).

