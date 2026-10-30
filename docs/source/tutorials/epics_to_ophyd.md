# From EPICS PVs To Ophyd Devices

This page is for users who know EPICS PVs, `caget`, `caput`, `camonitor`, and MEDM.

## PV Strings In SPEC

In SPEC, a PV is addressed directly:

```text
SPEC> epics_get("30idc:m26.RBV")
SPEC> epics_put("30idc:m26.VAL", 1.0)
```

## Devices In Bluesky

Bluesky wraps related PVs in ophyd objects. For example, `herix.th` represents an axis of the E4CH diffractometer:

```python
herix.th.position
herix.th.user_readback.get()
herix.th.user_setpoint.put(1.0)
```

An `EpicsMotor` wraps more than `.VAL` and `.RBV`; it also knows about motion completion, stop, limits, and engineering units. A plan can use the object without knowing each underlying PV:

```python
RE(bps.mv(herix.th, 1.0))
```

## Signals Versus Positioners

- `ophyd.EpicsMotor` is a positioner backed by an EPICS motor record.
- `ophyd.EpicsSignal` represents a general read/write PV.
- `ophyd.EpicsSignalRO` represents a read-only PV.
- `PVPositioner` represents a setpoint/readback device that is not an EPICS motor record.

The HKB table fields are represented as signals, not motors. The Kohzu monochromator uses a specialized positioner class because its energy motion requires setpoint, readback, move-command, moving, and stop PVs.

## Reading Values

One signal:

```python
herix.th.user_readback.get()
```

One device snapshot:

```python
herix.read()
kohzu.read()
```

Connection state:

```python
herix.connected
herix.wait_for_connection(timeout=5)
```

## Device Names And Data Names

Use dotted names to control devices in Python:

```python
herix.th.user_readback
```

Run data uses flat signal names, commonly with underscores:

```python
run.primary.read()
```

Trust the signal's `.name` when looking up a value in stored data:

```python
herix.th.user_readback.name
```

## One Connection Per PV

A device declaration creates persistent ophyd connections. Avoid declaring the same PV independently and inside a bundle. The current configuration comments out standalone alternatives when a motor is owned by a bundle or by the `herix` HKL device.

## Inspecting A Device

```python
herix.summary()
kohzu.summary()
herix.component_names
herix.read_attrs
herix.configuration_attrs
```

Use `%wa` to list the registered inventory.
