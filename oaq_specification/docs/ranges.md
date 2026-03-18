# Ranges

Range types represent ordered collections of values. They are used as output registers, loop sources, and post-processing inputs.

For a summary of the range type hierarchy, see [Abstract Types](./base_types.md).

---

## Numeric Ranges

### LiteralNumericRange

Defines a range of literal numeric values.

**Inherits from:** [`NumericRangeExpression`](./base_types.md#numericrangeexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Values | [NumericExpression](./base_types.md#numericexpression)[] | The values in the range. |

### NumericRangeRegister

Defines a named register that accumulates a range of numeric values during job execution.

**Inherits from:** [`NumericRangeExpression`](./base_types.md#numericrangeexpression)

### SteppedRange

Defines a linearly increasing numeric range in terms of a start, step, and stop value.

**Inherits from:** [`NumericRangeExpression`](./base_types.md#numericrangeexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| InclusiveStart | [NumericExpression](./base_types.md#numericexpression) | First value of the range. |
| Step | [NumericExpression](./base_types.md#numericexpression) | Increment between range values. |
| ExclusiveEnd | [NumericExpression](./base_types.md#numericexpression) | The range ends when a value would otherwise reach or exceed this number. |

### NumericAverageOver

Defines an averaging operation on a numeric range.

**Inherits from:** [`NumericRangeExpression`](./base_types.md#numericrangeexpression)

This expression is valid as an input for `Job.NumericRangeOutputs`. The user specifies which axes to average over by first reshaping the input range with `BufferDimensions` and then selecting the averaging axes with `AveragingAxis`.

#### Fields

| name |  type | description |
|------|-------|-------------|
| Name | string | The name of the resulting register. |
| Range | [NumericRangeExpression](./base_types.md#numericrangeexpression) | The range to average over. |
| BufferDimensions | [NumericLiteral](./expressions.md#numericliteral)[] | Defines the number of axes and their sizes. |
| AveragingAxis | [NumericLiteral](./expressions.md#numericliteral) | Defines which axis index to average over. |

### NumericRangeElement

Defines an indexing operation on a numeric range, producing a scalar numeric value.

**Inherits from:** [`NumericExpression`](./base_types.md#numericexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Source | [NumericRangeExpression](./base_types.md#numericrangeexpression) | The range to index. |
| Index | [NumericExpression](./base_types.md#numericexpression) | Index value; must be a non-negative integer. |

---

## Complex Ranges

### LiteralComplexRange

Defines a range of literal complex values.

**Inherits from:** [`ComplexRangeExpression`](./base_types.md#complexrangeexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Values | [Complex](./types.md#complex)[] | The values in the range. |

### ComplexRangeRegister

Defines a named register that accumulates a range of complex values during job execution.

**Inherits from:** [`ComplexRangeExpression`](./base_types.md#complexrangeexpression)

### AcquisitionComplexRangeResult

Defines the register holding the raw results of an ADC acquisition.

**Inherits from:** [`ComplexRangeExpression`](./base_types.md#complexrangeexpression)

Can be used as input for post-processing, e.g. demodulation via `Demodulation` or `ComplexDotProduct`.

### Demodulation

Defines a demodulation operation on an ADC acquisition with respect to a specified frame.

**Inherits from:** [`ComplexRangeExpression`](./base_types.md#complexrangeexpression)

The demodulation frequency and phase are taken from the specified frame.

#### Fields

| name |  type | description |
|------|-------|-------------|
| Trace | [AcquisitionComplexRangeResult](#acquisitioncomplexrangeresult) | The time trace to demodulate. |
| Frame | [Frame](./channels.md#frame) | The frame to demodulate the trace with respect to. |

### ComplexAverageOver

Defines an averaging operation on a complex range.

**Inherits from:** [`ComplexRangeExpression`](./base_types.md#complexrangeexpression)

This expression is valid as an input for `Job.ComplexRangeOutputs`. The user specifies which axes to average over by first reshaping the input range with `BufferDimensions` and then selecting the averaging axes with `AveragingAxis`.

#### Fields

| name |  type | description |
|------|-------|-------------|
| Name | string | The name of the resulting register. |
| Range | [ComplexRangeExpression](./base_types.md#complexrangeexpression) | The range to average over. |
| BufferDimensions | [NumericLiteral](./expressions.md#numericliteral)[] | Defines the number of axes and their sizes. |
| AveragingAxis | [NumericLiteral](./expressions.md#numericliteral) | Defines which axis index to average over. |

### ComplexRangeElement

Defines an indexing operation on a complex range, producing a scalar complex value.

**Inherits from:** [`ComplexExpression`](./base_types.md#complexexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Source | [ComplexRangeExpression](./base_types.md#complexrangeexpression) | The range to index. |
| Index | [NumericExpression](./base_types.md#numericexpression) | Index value; must be a non-negative integer. |

---

## Boolean Ranges

### BooleanRangeRegister

Defines a named register that accumulates a range of boolean values during job execution.

**Inherits from:** [`BooleanRangeExpression`](./base_types.md#booleanrangeexpression)

### BooleanRangeElement

Defines an indexing operation on a boolean range, producing a scalar boolean value.

**Inherits from:** [`BooleanExpression`](./base_types.md#booleanexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Source | [BooleanRangeExpression](./base_types.md#booleanrangeexpression) | The range to index. |
| Index | [NumericExpression](./base_types.md#numericexpression) | Index value; must be a non-negative integer. |

---

## Loop Constructs

### ForEachNumeric

Defines a for-loop that iterates a block body over a numeric range.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

The `Relationship` field determines how the block of each iteration is aligned with the next:

- `EndToStart` alignment produces a serial loop where each iteration begins after the previous completes
- `StartToStart` alignment produces a parallel loop where iterations can run simultaneously

#### Fields

| name |  type | description |
|------|-------|-------------|
| Source | [NumericRangeExpression](./base_types.md#numericrangeexpression) | The numeric range to iterate over. |
| Relationship | [InstructionRelationship](./timing.md#instructionrelationship) | The alignment relationship between consecutive iterations. |
| Body | [NumericToBlockFunction](./functions.md#numerictoblockfunction) | The body of the loop. |
