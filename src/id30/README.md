# `id30` Package

This package contains the APS 30-ID HERIX Bluesky instrument session.

- `startup.py` starts the RunEngine session and loads the instrument configuration.
- `configs/devices.yml` declares the HERIX devices.
- `configs/iconfig.yml` configures metadata, callbacks, and data output.
- `plans/` contains simulator verification plans; HERIX-specific user plans are still being developed.
- `callbacks/` contains the SPEC and NeXus callback examples used by the session.

For user-facing instructions, start with the repository [README](../../README.md), then read the [first-session tutorial](../../docs/source/tutorials/first_session.md) and [SPEC-to-Bluesky cross-walk](../../docs/source/tutorials/spec_to_bluesky.md).
