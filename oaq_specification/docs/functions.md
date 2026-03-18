# Functions

## NumericParameter
Defines a parameter with a numeric value which can be updated as part of a function.

**Inherits from:** [`NumericExpression`](./base_types.md#numericexpression)


## NumericToBlockFunction
Defines a function that takes in one numerical input parameter and resolves a Block with fields that contain the specified parameter.

Note that it's valid to parameterize any descendent blocks if the a Dependency is used as the input Block.

### Fields

| name |  type | description |
|------|-------|-------------|
| Name | string? | ID of the function
| Parameter | [NumericParameter](#numericparameter) | Non-literal symbol to be used in one or more of the Block fields.
| Body | [Block](./block.md#block) | The Block to evaluate that contains the NumericParameter in one or more valid fields.
