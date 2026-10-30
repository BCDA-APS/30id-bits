# HERIX Bluesky Cheat Sheet

## Related Resources

Official documentation: [Bluesky](https://blueskyproject.io/bluesky/), [ophyd](https://blueskyproject.io/ophyd/), [apstools](https://bcda-aps.github.io/apstools/latest/), [Tiled](https://blueskyproject.io/tiled/), [areaDetector/ADCore](https://areadetector.github.io/areaDetector/), and [hklpy2](https://blueskyproject.io/hklpy2/).

For advanced interactive questions, see the corresponding [DeepWiki resources](https://deepwiki.com/bluesky/bluesky), [ophyd](https://deepwiki.com/bluesky/ophyd), [apstools](https://deepwiki.com/BCDA-APS/apstools), [Tiled](https://deepwiki.com/bluesky/tiled), [areaDetector/ADCore](https://deepwiki.com/areaDetector/ADCore), and [hklpy2](https://deepwiki.com/bluesky/hklpy2). Verify released behavior against the official documentation.

## HKL Reference

The HERIX diffractometer uses hklpy2 with E4CH geometry. See the [official hklpy2 documentation](https://blueskyproject.io/hklpy2/) for geometry, solver, orientation, and reciprocal-space workflows.

## Start A Session

```bash
conda activate 30id-bits
ipython -i -c "from id30.startup import *"
```

## Find Devices

```python
%wa
herix
kohzu
scaler1
sim_motor
sim_det
```

Useful diffractometer axes:

```python
herix.th
herix.chi
herix.phi
herix.tth
```

Kohzu controls:

```python
kohzu.energy
kohzu.theta
kohzu.wavelength
```

## Move

```python
RE(bps.mv(herix.th, 1.0))
RE(bps.mv(herix.th, 1.0, herix.chi, 2.0))
RE(bps.mvr(herix.th, 0.1))

herix.th.position
herix.th.user_readback.get()
herix.th.read()
```

A plan such as `bps.mv(...)` needs `RE(...)`. Direct reads and direct ophyd methods do not.

## Count And Scan

```python
RE(bp.count([scaler1]))
RE(bp.count([scaler1], num=10))
RE(bp.scan([scaler1], herix.th, 0, 10, 11))
RE(bp.rel_scan([scaler1], herix.th, -1, 1, 11))
RE(bp.grid_scan([scaler1], herix.th, 0, 10, 11, herix.chi, 0, 5, 6))
```

Bluesky uses points. Ten SPEC intervals generally become eleven Bluesky points.

## Restore Installation Confidence

Use the simulator plans when checking a new or repaired installation without moving beamline hardware:

```python
RE(sim_print_plan())
RE(sim_count_plan())
RE(sim_rel_scan_plan())
```

## Inspect Data

```python
run = cat[-1]
run.metadata["start"]
run.primary.read()
run.baseline.read()
```

Use a signal's `.name` to find its stored data key:

```python
herix.th.user_readback.name
```

## Pause And Stop

```python
RE.resume()
RE.stop()
RE.abort()
RE.halt()
```

Use these at the RunEngine pause prompt.

## SPEC Cross-Walk

| SPEC | Bluesky |
| --- | --- |
| `mv th 1` | `RE(bps.mv(herix.th, 1))` |
| `mvr th 0.1` | `RE(bps.mvr(herix.th, 0.1))` |
| `wm th` | `herix.th.position` |
| `wa` | `%wa` |
| `ct 1` | `RE(bp.count([scaler1]))` |
| `ascan th 0 10 10 1` | `RE(bp.scan([scaler1], herix.th, 0, 10, 11))` |
| `dscan th -1 1 10 1` | `RE(bp.rel_scan([scaler1], herix.th, -1, 1, 11))` |
