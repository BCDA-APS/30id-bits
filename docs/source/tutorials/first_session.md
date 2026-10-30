# Your First HERIX Bluesky Session

This walkthrough is for SPEC users who are starting a Bluesky session for the first time.

## Start A Session

With the workstation environment already configured, run:

```bash
conda activate 30id-bits
ipython -i -c "from id30.startup import *"
```

This starts IPython with the HERIX instrument loaded and leaves the console prompt ready for commands. Startup creates the RunEngine `RE`, loads devices from `devices.yml`, and enables the configured callbacks. A live hardware session requires access to the 30-ID EPICS network.

## See The Devices

Use the standard IPython magic:

```python
%wa
```

Useful objects in the current HERIX configuration include:

```python
herix       # E4CH diffractometer
kohzu       # Kohzu monochromator
scaler1     # 30-IDB scaler
sim_motor   # simulator motor
sim_det     # simulator detector
```

Inspect a device or its components with tab completion:

```python
herix.
kohzu.
scaler1.
```

For a more detailed device summary:

```python
herix.summary()
kohzu.summary()
```

## Move A Device

A Bluesky plan must be handed to the RunEngine:

```python
RE(bps.mv(herix.th, 0.0))
```

For direct interactive ophyd access, use the device method directly:

```python
herix.th.move(0.0)
herix.th.position
```

Do not omit `RE(...)` from a plan:

```python
bps.mv(herix.th, 0.0)  # creates a plan; does not execute it
```

## Run A Standard Scan

Standard Bluesky plans are available as `bp` and plan stubs as `bps`:

```python
RE(bp.count([sim_det]))
RE(bp.scan([sim_det], sim_motor, -1, 1, 11))
```

For a real detector and motor, replace the simulator objects with the appropriate connected devices after confirming their names and readiness.

## (RE)Verify An Installation

If a new workstation setup or software update needs a hardware-independent check, run the simulator plans:

```python
RE(sim_print_plan())
RE(sim_count_plan())
RE(sim_rel_scan_plan())
```

These verify the Python package and RunEngine. They do not verify connection to HERIX hardware and are not HERIX experiment plans.

## Inspect The Most Recent Run

```python
run = cat[-1]
run.metadata["start"]
run.primary.read()
```

The current instrument configuration enables the [SPEC-format callback](https://bcda-aps.github.io/apstools/latest/api/_filewriters.html#apstools.callbacks.spec_file_writer.SpecWriterCallback2). Use the local data-management procedures for locating the resulting files; see the apstools [SPEC file structure](https://bcda-aps.github.io/apstools/latest/api/_filewriters.html#spec-file-structure) reference for the file layout.

## Next Steps

- [SPEC to Bluesky](spec_to_bluesky.md)
- [Running scans](../how_to/run_a_scan.md)
- [Inspecting data](../how_to/inspect_data.md)
- [HERIX cheat sheet](../reference/cheat_sheet.md)
