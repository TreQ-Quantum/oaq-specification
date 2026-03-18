# Channels

## Port
Defines a physical address on a quantum device, to which signals may be sent for control, and from which signals may be recorded for readout.

More generally, a port may represent a line on a quantum device where signals are expected to be received from control hardware and/or where the device may emit signals to the control hardware.

A port is conceptually associated with one or more QPU device elements, e.g. specific qubits, resonators, and tunable couplers, also common drive lines and feed lines.

Each port should be associated with a set of allowed sample rates that the backend provider should define. Instructions on a channel that conflict absolutely with allowed sample rates will return an error.


### Fields

| name |  type | description |
|------|-------|-------------|
ID | [NumericExpression](./base_types.md#numericexpression) | The ID reference of the port. A list of valid Port IDs for a given backend should be specified by the backend provider within their documentation.


## Frame
A defines a reference frequency and a phase.

Modulated pulse instructions are associated with a frame, which is used to compute the modulation frequency and phase offset for the instruction at that point in the schedule.

Instructions that share the same frame cannot overlap in time, i.e. have an implicit ordering dependency that will be validated for at compile time.

A frame is associated with one and only one port, however a port can support multiple frames. The backend documentation should specify frequency ranges and other constraints on allowed frames that each channel can support.

The phases of two frames can be synchronized, such that absolute phase differences between the two frames at any given time is deterministic. Currently this is hardcoded by the backend provider and should be specified in the documentation.

To support setups that rely on up-conversion, an optional intermediate frequency can be specified. A null value means that the backend can decide on the appropriate value to use, however a user may want to explicitly define the value if it's needed for calibration or for readout.

### Fields

| name |  type | description |
|------|-------|-------------|
Port | [Port](#port) | The Port this Frame is associated with. |
Frequency | [Frequency](./types.md#frequency) | The frequency that this reference frame should begin with.
Phase | [Angle](./types.md#angle) | The phase that this reference frame should begin with.
IntermediateFrequency | [Frequency](./types.md#frequency)? | Optional IF, if a value is given and is allowed by the backend then this IF will be used for up-conversion of instructions played on this Frame. If None, then the backend provider will take responsibility for choosing an appropriate IF if required.

## IirFilter
Defines an infinite impulse response filter to be applied to a channel.

The filter is defined according to:

$u(t) = a\exp\left( - \frac{t}{\tau}\right)$

where

* $a=\text{Amplitude}$
* $\tau=\text{TimeConstant}$

### Fields

| name |  type | description |
|------|-------|-------------|
| Amplitude | [Amplitude](./types.md#amplitude) |  The amplitude coefficient of the filter.
| TimeConstant | [Interval](./types.md#interval) | The time constant of the filter.


## FirFilter
Describes a finite impulse response filter to be applied to a channel.

The filter is defined by a range of real coefficients.

### Fields

| name |  type | description |
|------|-------|-------------|
| Taps | [LiteralNumericRange](./ranges.md#literalnumericrange) | The coefficients of the FIR filter to apply to the output.
