## Job
Top-level node that defines the inputs and outputs of the low-level job.

!!! note
    A valid implementation of the specification must contain this as the root node.

### Fields

| name |  type | description |
|------|-------|-------------|
| EntryPoint | [Block](./block.md#block) | Start of the program, which is a list of Instructions, Dependencies and/or ForEach statements.
| BooleanRangeOutputs | Dictionary&lt;string, [BooleanRangeRegister](./ranges.md#booleanrangeregister)&gt; | Mapping of names to Boolean range registers to be returned  on completion.
| NumericRangeOutputs | Dictionary&lt;string, [NumericRangeRegister](./ranges.md#numericrangeregister)&gt; | Mapping of names to Numeric range registers to be returned  on completion.
| ComplexRangeOutputs | Dictionary&lt;string, [ComplexRangeRegister](./ranges.md#complexrangeregister)&gt; | Mapping of names to Complex  range registers to be returned on completion.
