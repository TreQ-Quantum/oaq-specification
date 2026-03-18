# Drive Instructions

**Inherits from:** All drive instruction types inherit from [`Instruction`](./base_types.md#instruction).

## UnmodulatedPulse

Plays a waveform on a specified port.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

!!! note
    Unmodulated pulses played on the same port have an implicit end-start ordering dependency, i.e. they cannot overlap in time. Any program that conflicts with this should return an error during validation.

### Fields

| name |  type | description |
|------|-------|-------------|
| Envelope | [Waveform](./base_types.md#waveform) | The target waveform to play. |
| Port | [Port](./channels.md#port) | The port to play the waveform on. |
| Amplitude | [Amplitude](./types.md#amplitude) | The amplitude scale-factor multiplied with all envelope waveform values. |


## ModulatedPulse

Plays a waveform on a specified frame.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

The supplied envelope $u(t)$ will be modulated by the carrier frequency $f$ and phase $\phi$ defined in the associated frame, as well as the additional offset phase $\phi_{off}$ with the form:

$u_{mod}(t)=u(t)e^{i\left(2\pi f t - \left(\phi + \phi_{off}\right)\right)}$

!!! note
    Modulated pulses played on the same frame have an implicit end-start ordering dependency, i.e. they cannot overlap in time. Any program that conflicts with this should return an error during validation.

    Modulated pulses on different frames that share the same port have no implicit dependencies and can overlap in time. The amplitudes of these pulses will be summed together, and users should be careful that the combined amplitudes do not exceed any backend limits.

### Fields

| name |  type | description |
|------|-------|-------------|
| Envelope | [Waveform](./base_types.md#waveform) | The envelope of the waveform to be modulated and played. |
| Frame | [Frame](./channels.md#frame) | The frame to play the waveform on. |
| PhaseOffset | [Angle](./types.md#angle) | Additional phase offset to apply to the pulse modulation, for example to define rotations around different axes of the Bloch sphere. |


## Delay

Plays an explicit delay in the program.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

The specified delay is the minimum value and may increase depending on backend constraints such as sample rate.

!!! note
    Any DC biases will still be held during a Delay instruction.

### Fields

| name |  type | description |
|------|-------|-------------|
| Duration | [Interval](./types.md#interval) | Minimum amount of time to wait. |


## DcBias

Sets a static DC offset on the specified port.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

The bias value is held for the remainder of the job, or until another `DcBias` on the same port is played. The bias is additive in amplitude to all other instructions played on the same port.

It is up to the backend provider what the units of the offset correspond to (e.g. volts, amps).

!!! note
    The backend documentation should provide information on whether this operation has a duration and if so how long it will take to set the bias.

### Fields

| name |  type | description |
|------|-------|-------------|
| Port | [Port](./channels.md#port) | The port on which to set the bias offset. |
| Amplitude | [Amplitude](./types.md#amplitude) | The value of the bias to set. |


## SetFramePhase

Sets the phase of the specified frame to the target value at the present time in the schedule.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Frame | [Frame](./channels.md#frame) | The frame to update. |
| Phase | [Angle](./types.md#angle) | New phase value the frame will take. |

## ShiftFramePhase

Adds a phase offset to the specified frame at the present time in the schedule.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Frame | [Frame](./channels.md#frame) | The frame to update. |
| Phase | [Angle](./types.md#angle) | Additional phase to add to the current frame phase. |

## DetuneFrame

Updates the frame frequency with the specified detuning for all instructions played in the given scope.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Frame | [Frame](./channels.md#frame) | The frame to update. |
| Detuning | [Frequency](./types.md#frequency) | The frequency to detune the frame by. |
| Scope | [Block](./block.md#block) | The scope of instructions for which the frame is detuned. |
| PhaseReference | [PhaseReferencePoints](#phasereferencepoints) | Defines the point at which $t=0$ should be taken for the new frequency's phase. |


## PhaseReferencePoints

An enumeration defining possible reference points to use as $t=0$.

* `T0IsJobStart` — $t_0$ for the new frame oscillation is set to the start of the program.

* `T0IsNow` — $t_0$ for the new frame oscillation is set to the time at which the frame is updated.

## FilterOutput

Sets output filters on a port for all instructions in the given scope.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Port | [Port](./channels.md#port) | The port to apply output filters to. Note: not all ports will support filters. |
| Scope | [Block](./block.md#block) | The scope of instructions for which the output filter is set. |
| IIRFilters | [IIRFilter](./channels.md#iirfilter)[] | An array of IIR filters to apply to the output. |
| FIRFilter | [FIRFilter](./channels.md#firfilter) | The FIR filter to apply to the output. |

## BlockConditional

Conditionally branches a program of instructions.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Guard | [BooleanExpression](./base_types.md#booleanexpression) | Determines the branch to take. |
| TrueCase | [Block](./block.md#block) | Sub-program to play if the guard is true. |
| FalseCase | [Block](./block.md#block) | Sub-program to play if the guard is false. |
