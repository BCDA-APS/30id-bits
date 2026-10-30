# How To Run A Scan

Bluesky standard plans cover the common SPEC scan patterns while custom HERIX plans are being developed.

## Invocation Pattern

Plans are called through the RunEngine and normally receive a list of detectors first:

```python
RE(bp.count([scaler1]))
RE(bp.scan([scaler1], herix.th, 0, 10, 11))
RE(bp.rel_scan([scaler1], herix.th, -1, 1, 11))
RE(bp.grid_scan([scaler1], herix.th, 0, 10, 11, herix.chi, 0, 5, 6))
RE(bp.list_scan([scaler1], herix.th, [0, 1, 2, 3]))
```

## SPEC Point Counts

SPEC commonly specifies intervals. Bluesky specifies points:

```text
SPEC:     ascan th 0 10 10 1
Bluesky:  bp.scan([scaler1], herix.th, 0, 10, 11)
```

Ten intervals produce eleven points.

## Counts

```python
RE(bp.count([scaler1]))
RE(bp.count([scaler1], num=10))
RE(bp.count([scaler1], num=10, delay=0.5))
```

Use the detector object appropriate to the measurement. The scaler and Pilatus configuration are still being integrated with the experiment-specific acquisition workflow.

## Metadata

Attach sample and purpose information to a run:

```python
RE(bp.scan(
    [scaler1],
    herix.th,
    0,
    10,
    11,
    md={
        "sample": "sample name",
        "purpose": "alignment",
    },
))
```

## Pause And Stop

During a plan:

- `Ctrl-C` requests a pause.
- `RE.resume()` continues a paused plan.
- `RE.stop()` ends it cleanly.
- `RE.abort()` ends it with an abort status.
- `RE.halt()` is an emergency stop without normal document completion.

## Custom Plans

Commonly-repeated HERIX workflows should become plans in `src/id30/plans/` rather than copied IPython command sequences. The repository does not yet define those user-facing HERIX plans, so standard `bluesky.plans` are the current reference implementation.
