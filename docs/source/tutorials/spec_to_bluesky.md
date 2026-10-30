# Coming To Bluesky From SPEC

This page assumes you know SPEC commands such as `mv`, `umv`, `mvr`, `ascan`, `dscan`, `ct`, and `wm`.

## The Important Difference

In SPEC, a command executes when entered:

```text
SPEC> mv th 1.0
```

In Bluesky, a plan describes the operation and the RunEngine executes it:

```python
RE(bps.mv(herix.th, 1.0))
```

`bps.mv(...)` returns a plan object. `RE(...)` executes it.

## Command Cross-Walk

| SPEC | Bluesky | Meaning |
| --- | --- | --- |
| `mv th 1` | `RE(bps.mv(herix.th, 1))` | Absolute move |
| `mv th 1 chi 2` | `RE(bps.mv(herix.th, 1, herix.chi, 2))` | Parallel move |
| `mvr th 0.1` | `RE(bps.mvr(herix.th, 0.1))` | Relative move |
| `wm th` | `herix.th.position` | Current position |
| `wa` | `%wa` | List registered devices |
| `ct 1` | `RE(bp.count([scaler1]))` | One count |
| `ct 1 5` | `RE(bp.count([scaler1], num=5))` | Five counts |
| `ascan th 0 10 10 1` | `RE(bp.scan([scaler1], herix.th, 0, 10, 11))` | Absolute scan |
| `dscan th -1 1 10 1` | `RE(bp.rel_scan([scaler1], herix.th, -1, 1, 11))` | Relative scan |
| `a2scan th 0 10 chi 0 5 10 1` | `RE(bp.scan([scaler1], herix.th, 0, 10, herix.chi, 0, 5, 11))` | Synchronized scan |
| `mesh th 0 10 5 chi 0 5 5 1` | `RE(bp.grid_scan([scaler1], herix.th, 0, 10, 6, herix.chi, 0, 5, 6))` | Mesh scan |
| `qdo file.mac` | `%run -i file.py` | Load Python code |

Bluesky scan arguments use **points**. SPEC scan commands usually specify **intervals**, so add one to the SPEC interval count.

## The E4CH Diffractometer

The current declarative configuration exposes the HERIX [E4CH geometry](https://blueskyproject.io/hklpy2/diffractometers.html#geometries-hkl-soleil-e4ch) diffractometer as `herix`:

```python
herix.th
herix.chi
herix.phi
herix.tth
```

The HKL calculation and orientation workflow is still being developed. Do not assume that every historical SPEC macro has a direct equivalent yet.

For the hklpy2 concepts, geometry, reciprocal-space calculations, orientation, and solver workflows, see the [hklpy2 documentation](https://blueskyproject.io/hklpy2/). An additional community-maintained AI-assisted overview is available at [DeepWiki: hklpy2](https://deepwiki.com/bluesky/hklpy2); verify operational details against the official documentation and this instrument configuration.

## Kohzu Energy

The Kohzu monochromator is exposed as `kohzu`:

```python
kohzu.energy
kohzu.theta
kohzu.wavelength
```

The device wraps the Kohzu sequence-control PVs rather than treating `kohzuE` as an ordinary EPICS motor.

## Plans And Plan Stubs

- `bp.*` contains complete plans such as `bp.count`, `bp.scan`, and `bp.grid_scan`.
- `bps.*` contains plan stubs such as `bps.mv`, `bps.mvr`, and `bps.read`.
- Both are executed through `RE(...)` at the prompt.

## When Not To Use `RE(...)`

Direct data access is not a plan:

```python
herix.th.position
herix.th.user_readback.get()
herix.th.read()
kohzu.read()
```

Use `RE(...)` for a generator/plan. Call methods such as `.get()`, `.read()`, and `.position` directly when inspecting data.

## What Is Different From SPEC

Bluesky provides structured run documents, pause/resume, callbacks, and reusable plans. The tradeoff is more verbose commands and a Python object model instead of a short command language.

Custom HERIX plans have not been sketched out yet. Until they are, standard Bluesky plans are the supported starting point.
