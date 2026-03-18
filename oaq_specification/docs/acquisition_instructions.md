# Acquisition Instructions

**Inherits from:** All acquisition instruction types inherit from [`Instruction`](./base_types.md#instruction).

## AdcAcquisition

Defines an acquisition of a voltage time-trace via an ADC.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Port | [Port](./channels.md#port) | The port on which to acquire a signal. |
| Duration | [Interval](./types.md#interval) | The length of time to read values for. |
| Result | [AcquisitionComplexRangeResult](./ranges.md#acquisitioncomplexrangeresult) | The results register where the acquired trace is stored. |


## NumericAppend

Defines an append operation of a numeric value onto a register.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Input | [NumericExpression](./base_types.md#numericexpression) | The value to append. |
| Output | [NumericRangeRegister](./ranges.md#numericrangeregister) | The register to append to. |


## ComplexAppend

Defines an append operation of a complex value onto a register.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Input | [ComplexExpression](./base_types.md#complexexpression) | The value to append. |
| Output | [ComplexRangeRegister](./ranges.md#complexrangeregister) | The register to append to. |


## BooleanAppend

Defines an append operation of a boolean value onto a register.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Input | [BooleanExpression](./base_types.md#booleanexpression) | The value to append. |
| Output | [BooleanRangeRegister](./ranges.md#booleanrangeregister) | The register to append to. |


## ComplexRangeAssign

Defines an assignment of a complex range to a complex range register.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

### Fields

| name |  type | description |
|------|-------|-------------|
| Input | [ComplexRangeExpression](./base_types.md#complexrangeexpression) | The complex range to assign. |
| Output | [ComplexRangeRegister](./ranges.md#complexrangeregister) | The register to assign to. |
