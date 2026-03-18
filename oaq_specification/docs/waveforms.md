# Waveforms

Several template waveforms can be expressed in the spec, as well as explicitly defined numerical waveforms. In addition, waveforms can be given a plateau or a DRAG modification with the encapsulating modifier classes defined below.

**Inherits from:** All waveform types inherit from [`Waveform`](./base_types.md#waveform).

## BlackmanWaveform

Defines a waveform with a [Blackman](https://mathworld.wolfram.com/BlackmanFunction.html) pulse shape.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

The waveform is defined according to the formula

$u(t)=a_0+2\sum{}_{n=1}^\infty a_n \cos \left(\frac{2n\pi x}{d}\right)$

where

* $d = \text{Duration}$
* $x=t-t_0$
* $t_0=\frac{d}{2}$
* $a_0=\frac{3969}{9304}$
* $a_1=\frac{1155}{4652}$
* $a_2=\frac{715}{18608}$

Typically taken up to $n=2$.

### Fields

| name |  type | description |
|------|-------|-------------|
| Duration | [Interval](./types.md#interval) | The duration of the waveform. |

## ConstantWaveform

Defines a constant unit-amplitude value to be held for a duration.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

The waveform is defined by the formula $u(t)=1$. The effective output amplitude is set by the `Amplitude` field on the pulse instruction that references this waveform.

### Fields

| name |  type | description |
|------|-------|-------------|
| Duration | [Interval](./types.md#interval) | The duration of the waveform. |

## CosineWaveform

Defines a waveform that takes on the shape of a single period of an inverted and offset cosine.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

The waveform is defined according to the formula

$u(t)=\frac{1}{2}\left( 1+\cos{\left( \frac{2\pi x}{d}\right)}\right)$

* $d = \text{Duration}$
* $x=t-t_0$
* $t_0=\frac{d}{2}$

### Fields

| name |  type | description |
|------|-------|-------------|
| Duration | [Interval](./types.md#interval) | The duration of the waveform. |

## GaussianWaveform

Defines a truncated Gaussian pulse shape.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

The waveform is defined according to the formula

$u(t) = \exp\left( - \frac{\left(t-t_{0}\right)^2}{2\sigma^{2}}\right)$

where

* $d = \text{Duration}$
* $\sigma=\text{Sigma}$
* $t_0=\frac{d}{2}$

### Fields

| name |  type | description |
|------|-------|-------------|
| Duration | [Interval](./types.md#interval) | The duration of the waveform. The Gaussian will be truncated to [0, Duration]. |
| Sigma | [Interval](./types.md#interval) | The standard deviation of the Gaussian. |

## SampledNumericWaveform

Defines a real-valued waveform from an explicit array of samples.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

The envelope is defined by an array of numeric values and a time step. The time step refers to the start of each sample — for a step of 1 ns, the first sample is held from 0–1 ns, the second from 1–2 ns, and so on.

### Fields

| name |  type | description |
|------|-------|-------------|
| SampleInterval | [Interval](./types.md#interval) | The time resolution of the waveform $dt$. |
| Samples | [NumericRangeExpression](./base_types.md#numericrangeexpression) | The waveform values. |

## SampledComplexWaveform

Defines a complex-valued waveform from an explicit array of samples.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

The envelope is defined by an array of complex values and a time step. The time step refers to the start of each sample — for a step of 1 ns, the first sample is held from 0–1 ns, the second from 1–2 ns, and so on.

### Fields

| name |  type | description |
|------|-------|-------------|
| SampleInterval | [Interval](./types.md#interval) | The time resolution of the waveform $dt$. |
| Samples | [ComplexRangeExpression](./base_types.md#complexrangeexpression) | The waveform values. |

## SuddenNetZeroWaveform

Defines a [sudden net-zero](https://arxiv.org/pdf/2008.07411) waveform, commonly used for two-qubit gates in superconducting architectures.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

The waveform is defined according to the piece-wise formula:

$$
u(t) =
\begin{cases}
1,  & \text{for } 0                                 \le t < t_p \\[1ex]
b, & \text{for }  t_p                               \le t < \left( t_p + t_b \right) \\[1ex]
0, & \text{for }  \left( t_p + t_b \right)          \le t < \left( t_p + t_b + t_\phi \right) \\[1ex]
-b, & \text{for } \left( t_p + t_b + t_{\phi} \right) \le t < \left( t_p + 2t_b + t_\phi \right) \\[1ex]
-1, & \text{for } \left( t_p + 2t_b + t_{\phi} \right) \le t < \left(2t_p + 2t_b + t_\phi \right)
\end{cases}
$$

where

* $t_p = \text{Duration}/2$
* $t_b=\text{BDuration}$
* $t_\phi=\text{MidPointDelay}$
* $b=\text{BAmplitude}$

### Fields

| name |  type | description |
|------|-------|-------------|
| Duration | [Interval](./types.md#interval) | The overall duration of the square part of the waveform. Note that the positive and negative parts of the pulse each last for half of this duration. |
| MidpointDelay | [Interval](./types.md#interval) | The delay between the positive and negative parts of the net-zero pulse, $t_\phi$. |
| BDuration | [Interval](./types.md#interval) | Duration of the fine-adjustment step between the positive/negative pulses and the midpoint delay. This duration applies to both the rise and fall steps. |
| BAmplitude | [Amplitude](./types.md#amplitude) | The relative amplitude of the fine-adjustment steps. A value of 0.5 sets the fine-adjustment amplitude to half that of the net-zero pulses. |

## DragWaveform

Applies a first-order DRAG correction to a base waveform.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

!!! note
    `DragWaveform` is a **modifier wrapper** — it transforms an existing waveform rather than defining a standalone shape. The `Base` field must reference another `Waveform`.

The provided waveform $u(t)$ is updated to use the following form:

$u_{\text{DRAG}}(t)=u_x(t) + i\beta\frac{du_x(t)}{dt}$

where

* $u_x(t) = \Re\lbrace\text{Waveform}\rbrace$
* $\beta=\text{DragParameter}$

### Fields

| name |  type | description |
|------|-------|-------------|
| Base | [Waveform](./base_types.md#waveform) | The waveform to which to apply the DRAG correction. |
| DragParameter | [Interval](./types.md#interval) | The DRAG correction parameter, in dimensions of time. This combines the dimensionless DRAG scaling parameter divided by the anharmonicity. |

## PlateauWaveform

Applies a central-plateau modification to a base waveform.

**Inherits from:** [`Waveform`](./base_types.md#waveform)

!!! note
    `PlateauWaveform` is a **modifier wrapper** — it transforms an existing waveform rather than defining a standalone shape. The `Base` field must reference another `Waveform`.

The waveform is updated to include a constant amplitude portion of the specified duration inserted at the midpoint of the waveform array:

* For a waveform that evaluates to an even number of samples $n$, the constant portion is inserted between $n/2$ and $n/2 + 1$, with amplitude matching the left-hand sample at $n/2$.
* For a waveform that evaluates to an odd number of samples $n$, the constant portion is inserted after the middle value, between $\frac{n}{2} + 0.5$ and $\frac{n}{2} + 1.5$, with amplitude matching the left-hand sample at $\frac{n}{2} + 0.5$.

### Fields

| name |  type | description |
|------|-------|-------------|
| Base | [Waveform](./base_types.md#waveform) | The waveform to update with a central plateau. |
| Plateau | [Interval](./types.md#interval) | The duration to hold at a constant value at the midpoint of the waveform. |
