# 30-ID BITS instrument

[Bluesky Instrument (BITS)](https://github.com/BCDA-APS/BITS) package for APS
beamline 30-ID HERIX. It is built on the
[`apsbits`](https://github.com/BCDA-APS/apsbits) framework.

**NOTE**: This instrument is not yet configured to save Bluesky runs to a
persistent database.  For now, the only permanent record are the SPEC data
files.  Database support is expected to be ready before 2026-12-31.

## Where to start

- **New to Bluesky?** Read [Your first HERIX Bluesky session](docs/source/tutorials/first_session.md).
- **Coming from SPEC?** Read [The SPEC-to-Bluesky cross-walk](docs/source/tutorials/spec_to_bluesky.md).
- **Using the HERIX diffractometer?** See the [hklpy2 documentation](https://blueskyproject.io/hklpy2/).
- **Working with EPICS PVs?** Read [From EPICS PVs to ophyd devices](docs/source/tutorials/epics_to_ophyd.md).
- **Need a daily reference?** Use the [HERIX cheat sheet](docs/source/reference/cheat_sheet.md).

This documentation is written for users who already know SPEC. Bluesky-specific
concepts are introduced where they affect day-to-day operation.

## Quick Start

With the environment installed on the workstation:

```bash
conda activate 30id-bits
ipython -i -c "from id30.startup import *"
```

This starts IPython for a Bluesky session, ending with the console prompt.

In detail: Startup loads the RunEngine, devices, simulator plans, SPEC data-file
callback, and standard IPython helpers. Hardware connection requires access to
the 30-ID EPICS network.

## Current Instrument Scope

The configuration currently includes the HERIX E4CH diffractometer,
Kohzu monochromator, motor bundles, scaler, Pilatus area detector configuration,
and selected EPICS signals. HERIX-specific user plans are not yet defined; use
the standard Bluesky plans described in the documentation until those plans are
added.

The SPEC-format callback is enabled in the current configuration. The exact
output location and data-management behavior depend on the workstation and
session setup.

## Documentation

- [First session](docs/source/tutorials/first_session.md)
- [SPEC to Bluesky](docs/source/tutorials/spec_to_bluesky.md)
- [EPICS to ophyd](docs/source/tutorials/epics_to_ophyd.md)
- [Running scans](docs/source/how_to/run_a_scan.md)
- [Inspecting data](docs/source/how_to/inspect_data.md)
- [Cheat sheet](docs/source/reference/cheat_sheet.md)

## Queueserver

Queueserver support is included in the package, but beamline deployment and permissions are site-specific. The host script is:

```bash
./scripts/id30_qs_host.sh restart
```

Use the queue-monitor client from a workstation configured for the queue server:

```bash
queue-monitor &
```

For command details:

```bash
./scripts/id30_qs_host.sh help
```

## Configuration

- `src/id30/configs/devices.yml` declares instrument devices.
- `src/id30/configs/iconfig.yml` configures the RunEngine, callbacks, metadata, and data output.
- `src/id30/configs/devices_aps_only.yml` declares devices available only on the APS network.
- `src/id30/startup.py` loads the instrument session.
