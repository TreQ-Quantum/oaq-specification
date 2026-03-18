# Base Types

The OAQ specification is structured around a set of abstract base classes that define the major categories of objects in the abstract syntax graph (ASG). Every concrete type in the specification inherits from one of these bases.

Understanding the Base Types is the quickest way to orient yourself within the specification — they describe *what kind of thing* an object is before any details about its fields.

## Instruction

An `Instruction` is an executable step that can appear in a [Block](./block.md). All drive operations, acquisition operations, timing dependencies, and loop constructs are `Instruction` subtypes.

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [UnmodulatedPulse](./drive_instructions.md#unmodulatedpulse) | Plays a waveform on a port |
| [ModulatedPulse](./drive_instructions.md#modulatedpulse) | Plays a modulated waveform on a frame |
| [Delay](./drive_instructions.md#delay) | Inserts an explicit time delay |
| [DcBias](./drive_instructions.md#dcbias) | Sets a static DC offset on a port |
| [SetFramePhase](./drive_instructions.md#setframephase) | Sets the absolute phase of a frame |
| [ShiftFramePhase](./drive_instructions.md#shiftframephase) | Adds a phase offset to a frame |
| [DetuneFrame](./drive_instructions.md#detuneframe) | Updates a frame's frequency within a scoped block |
| [FilterOutput](./drive_instructions.md#filteroutput) | Applies IIR/FIR output filters within a scoped block |
| [BlockConditional](./drive_instructions.md#blockconditional) | Conditionally branches a program of instructions |
| [AdcAcquisition](./acquisition_instructions.md#adcacquisition) | Acquires a voltage time-trace via ADC |
| [NumericAppend](./acquisition_instructions.md#numericappend) | Appends a numeric value to a register |
| [ComplexAppend](./acquisition_instructions.md#complexappend) | Appends a complex value to a register |
| [BooleanAppend](./acquisition_instructions.md#booleanappend) | Appends a boolean value to a register |
| [ComplexRangeAssign](./acquisition_instructions.md#complexrangeassign) | Assigns a complex range to a register |
| [Dependency](./timing.md#dependency) | Specifies a timing dependency between two blocks |
| [ForEachNumeric](./ranges.md#foreachnumeric) | Iterates a block over a numeric range |

---

## Waveform

A `Waveform` defines a pulse envelope shape. Waveforms are referenced by pulse instructions (`UnmodulatedPulse`, `ModulatedPulse`) and by modifier wrappers (`DragWaveform`, `PlateauWaveform`).

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [BlackmanWaveform](./waveforms.md#blackmanwaveform) | Blackman pulse shape |
| [ConstantWaveform](./waveforms.md#constantwaveform) | Constant amplitude held for a duration |
| [CosineWaveform](./waveforms.md#cosinewaveform) | Single period of an inverted, offset cosine |
| [GaussianWaveform](./waveforms.md#gaussianwaveform) | Truncated Gaussian pulse |
| [SampledNumericWaveform](./waveforms.md#samplednumericwaveform) | Explicitly sampled real-valued waveform |
| [SampledComplexWaveform](./waveforms.md#sampledcomplexwaveform) | Explicitly sampled complex-valued waveform |
| [SuddenNetZeroWaveform](./waveforms.md#suddennetzerowaveform) | Sudden net-zero (SNZ) gate pulse |
| [DragWaveform](./waveforms.md#dragwaveform) | DRAG correction modifier applied to a base waveform |
| [PlateauWaveform](./waveforms.md#plateauwaveform) | Central plateau modifier applied to a base waveform |

---

## NumericExpression

A `NumericExpression` is a scalar real-valued expression. Numeric expressions appear as field values wherever a real number is expected (e.g. amplitudes, frequencies, loop indices).

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [NumericLiteral](./expressions.md#numericliteral) | A literal real number |
| [NumericConditional](./expressions.md#numericconditional) | Conditional numeric expression |
| [BinaryNumericOperation](./expressions.md#binarynumericoperation) | Binary arithmetic on two numeric expressions |
| [NumericRangeElement](./ranges.md#numericrangeelement) | Index into a numeric range to produce a scalar |
| [ComplexRealValue](./expressions.md#complexrealvalue) | Real part of a complex expression |
| [ComplexImaginaryValue](./expressions.md#compleximaginaryvalue) | Imaginary part of a complex expression |
| [NumericParameter](./functions.md) | A function parameter within a `NumericToBlockFunction` |

---

## ComplexExpression

A `ComplexExpression` is a scalar complex-valued expression.

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [BinaryComplexOperation](./expressions.md#binarycomplexoperation) | Binary arithmetic on two complex expressions |
| [ComplexConditional](./expressions.md#complexconditional) | Conditional complex expression |
| [ComplexDotProduct](./expressions.md#complexdotproduct) | Dot product between two complex ranges |
| [ComplexRangeElement](./ranges.md#complexrangeelement) | Index into a complex range to produce a scalar |

---

## BooleanExpression

A `BooleanExpression` is a scalar boolean-valued expression. Boolean expressions are used as guards in conditional instructions and conditional expressions.

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [ComparisonOperation](./expressions.md#comparisonoperation) | Comparison between two numeric expressions |
| [BooleanConditional](./expressions.md#booleanconditional) | Conditional boolean expression |
| [BooleanRangeElement](./ranges.md#booleanrangeelement) | Index into a boolean range to produce a scalar |

---

## NumericRangeExpression

A `NumericRangeExpression` is an array of numeric values. Range expressions are used as loop sources, output registers, and post-processing inputs.

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [LiteralNumericRange](./ranges.md#literalnumericrange) | An inline array of literal numeric values |
| [NumericRangeRegister](./ranges.md#numericrangeregister) | A named output register for numeric results |
| [SteppedRange](./ranges.md#steppedrange) | A linearly stepped range (start, step, end) |
| [NumericAverageOver](./ranges.md#numericaverageover) | Averaging aggregation over a numeric range |
| [ForEachNumeric](./ranges.md#foreachnumeric) | The source range for a `ForEachNumeric` loop |

---

## ComplexRangeExpression

A `ComplexRangeExpression` is an array of complex values. Used for acquisition results, demodulation, and output registers.

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [LiteralComplexRange](./ranges.md#literalcomplexrange) | An inline array of literal complex values |
| [ComplexRangeRegister](./ranges.md#complexrangeregister) | A named output register for complex results |
| [AcquisitionComplexRangeResult](./ranges.md#acquisitioncomplexrangeresult) | Holds the raw result of an ADC acquisition |
| [ComplexAverageOver](./ranges.md#complexaverageover) | Averaging aggregation over a complex range |
| [Demodulation](./ranges.md#demodulation) | Demodulated signal from an ADC trace |

---

## BooleanRangeExpression

A `BooleanRangeExpression` is an array of boolean values.

**Concrete subtypes:**

| Type | Description |
|------|-------------|
| [BooleanRangeRegister](./ranges.md#booleanrangeregister) | A named output register for boolean results |
