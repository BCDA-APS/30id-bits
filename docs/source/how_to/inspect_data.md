# How To Inspect Data

A Bluesky run is a collection of structured documents. The active 30-ID configuration enables the SPEC-format callback; catalog and Tiled behavior depend on the local deployment.

## Inspect The Current Run

After a plan completes:

```python
run = cat[-1]
run.metadata["start"]
run.metadata["stop"]
run.primary.read()
```

The primary stream contains the main event data from a scan or count.

## Metadata

Useful start metadata includes:

```python
run.metadata["start"]["scan_id"]
run.metadata["start"]["plan_name"]
run.metadata["start"]["detectors"]
run.metadata["start"]["motors"]
```

Add experiment-specific metadata with the plan's `md={...}` argument.

## Data Names

Python device paths use dots:

```python
herix.th.user_readback
```

Stored event data uses each signal's flat `.name`:

```python
signal_name = herix.th.user_readback.name
run.primary.read()[signal_name]
```

## Baseline Stream

Only explicitly selected baseline devices appear in a `baseline` stream. The current instrument configuration does not label the `D1` temperature signals as `baseline` yet. Motors are not automatically placed in the baseline stream because a separate SPEC-style motor-position subscriber is being considered for that purpose. Baseline membership can be revised after user feedback.

If a run contains baseline data:

```python
run.baseline.read()
```

At present, the instrument-specific baseline stream may be absent because no ordinary HERIX devices are selected for baseline capture.

## SPEC Output

`SPEC_DATA_FILES.ENABLE` is currently true in `iconfig.yml`. The SPEC callback is intended to provide a familiar text output path for SPEC users, but file location and data-management conventions are site-specific. Confirm the active output location with beamline staff.

## Common Pitfalls

- Do not wrap `run.primary.read()` in `RE(...)`; it returns data, not a plan.
- Do not read a large area-detector image eagerly unless you intend to load it into memory.
- A simulator run verifies the software stack, not hardware connectivity.
- If a device is disconnected, check EPICS network access before changing the plan.
