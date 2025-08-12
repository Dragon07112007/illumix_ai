# Patching File Format

Illumix stores fixture configuration in JSON. Two types of files are used:

## Fixture Preset
A fixture preset defines the name of a fixture and its component list. Example
for a single‑channel dimmer (`1kw.json`):

```json
{
    "name": "1kw",
    "components": [ { "Dimmer": { "intensity": 0 } } ]
}
```

Each element in `components` matches a variant of the
[`FixtureComponent`](../fixture_lib/src/fixture.rs) enum.

## Patching
`patching.json` lists the fixtures that make up a DMX universe. Each entry
provides a unique `id`, starting `dmx_address` and a path to the
corresponding fixture preset:

```json
{
    "fixtures": [
        { "dmx_address": 1, "id": 1, "fixture_preset": "patching/1kw.json" }
    ]
}
```

The backend loads this file at start‑up and uses
[`Patching::to_universe`](../fixture_lib/src/patching.rs) to construct the
runtime [`Universe`](../fixture_lib/src/universe.rs).

