# 30-ID BITS instrument

[Bluesky Instrument (BITS)](https://github.com/BCDA-APS/BITS) package for APS
beamline 30-ID HERIX. It is built on the
[`apsbits`](https://github.com/BCDA-APS/apsbits) framework.

**NOTE**: This instrument is not yet configured to save Bluesky runs to a
persistent database.  For now, the only permanent records are the SPEC data
files written during Bluesky runs.  Database support is expected to be ready before 2026-12-31.

## Where to start

- **New to Bluesky?** Read [Your first HERIX Bluesky session](docs/source/tutorials/first_session.md).
- **Coming from SPEC?** Read [The SPEC-to-Bluesky cross-walk](docs/source/tutorials/spec_to_bluesky.md).
- **Using the HERIX diffractometer?** See the [hklpy2 documentation](https://blueskyproject.io/hklpy2/).
- **Working with EPICS PVs?** Read [From EPICS PVs to ophyd devices](docs/source/tutorials/epics_to_ophyd.md).
- **Need a daily reference?** Use the [HERIX cheat sheet](docs/source/reference/cheat_sheet.md).

This documentation is written for users who already know SPEC. Bluesky-specific
concepts are introduced where they affect day-to-day operation.

## Related Resources

Official project documentation:

- [Bluesky](https://blueskyproject.io/bluesky/)
- [ophyd](https://blueskyproject.io/ophyd/)
- [apstools](https://bcda-aps.github.io/apstools/latest/)
- [Tiled](https://blueskyproject.io/tiled/)
- [areaDetector/ADCore](https://areadetector.github.io/areaDetector/)
- [hklpy2](https://blueskyproject.io/hklpy2/)

For advanced questions, the corresponding interactive AI resources are available through DeepWiki:

- [Bluesky on DeepWiki](https://deepwiki.com/bluesky/bluesky)
- [ophyd on DeepWiki](https://deepwiki.com/bluesky/ophyd)
- [apstools on DeepWiki](https://deepwiki.com/BCDA-APS/apstools)
- [Tiled on DeepWiki](https://deepwiki.com/bluesky/tiled)
- [areaDetector/ADCore on DeepWiki](https://deepwiki.com/areaDetector/ADCore)
- [hklpy2 on DeepWiki](https://deepwiki.com/bluesky/hklpy2)

Use the official documentation as the authority for released behavior and configuration details.

## Quick Start

With the environment installed on the workstation:

```bash
conda activate 30id-bits
ipython -i -c "from id30.startup import *"
```

This starts IPython for a Bluesky session, ending with the console prompt.

*Details*: Startup loads the RunEngine, devices, simulators, SPEC data-file
callback, and standard IPython helpers. Hardware connection requires access to
the 30-ID EPICS network.

## Current Instrument Scope

The configuration currently includes the HERIX E4CH diffractometer,
Kohzu monochromator, motor bundles, scaler, Pilatus area detector configuration,
and selected EPICS signals. HERIX-specific user plans are not yet defined; use
the standard Bluesky plans described in the documentation until those plans are
added.

The [SPEC-format callback](https://bcda-aps.github.io/apstools/latest/api/_filewriters.html#apstools.callbacks.spec_file_writer.SpecWriterCallback2) is enabled in the current configuration. The exact output location and data-management behavior depend on the workstation and session setup. See the apstools [SPEC file structure](https://bcda-aps.github.io/apstools/latest/api/_filewriters.html#spec-file-structure) reference for the produced file layout.

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
