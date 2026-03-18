# Expressions

Expression types represent computed values. They are used as field values throughout the specification wherever a numeric, complex, or boolean value is required.

For a summary of the expression type hierarchy, see [Abstract Types](./base_types.md).

---

## Numeric Expressions

### NumericLiteral

Defines a real-valued literal number.

**Inherits from:** [`NumericExpression`](./base_types.md#numericexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Value | double | The literal value. |


### ComplexRealValue

Isolates the real part of a complex expression.

**Inherits from:** [`NumericExpression`](./base_types.md#numericexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Operand | [ComplexExpression](./base_types.md#complexexpression) | The complex expression to take the real part of. |


### ComplexImaginaryValue

Isolates the imaginary part of a complex expression.

**Inherits from:** [`NumericExpression`](./base_types.md#numericexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Operand | [ComplexExpression](./base_types.md#complexexpression) | The complex expression to take the imaginary part of. |


### NumericConditional

A numeric conditional expression.

**Inherits from:** [`NumericExpression`](./base_types.md#numericexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Guard | [BooleanExpression](./base_types.md#booleanexpression) | Determines the case of the conditional. |
| TrueCase | [NumericExpression](./base_types.md#numericexpression) | Output if the guard is true. |
| FalseCase | [NumericExpression](./base_types.md#numericexpression) | Output if the guard is false. |


### BinaryNumericOperation

A binary operation between two numeric expressions.

**Inherits from:** [`NumericExpression`](./base_types.md#numericexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Operator | [BinaryNumericOperators](#binarynumericoperators) | Operation to perform. |
| Lhs | [NumericExpression](./base_types.md#numericexpression) | Left-hand side of the expression. |
| Rhs | [NumericExpression](./base_types.md#numericexpression) | Right-hand side of the expression. |

### BinaryNumericOperators

An enumeration for binary numeric operators.

* `Add` — addition: $a + b$
* `Subtract` — subtraction: $a - b$
* `Multiply` — multiplication: $a \times b$

---

## Complex Expressions

### ComplexDotProduct

Defines a dot product operation between two complex ranges.

**Inherits from:** [`ComplexExpression`](./base_types.md#complexexpression)

!!! note
    The two ranges must have the same size, otherwise an exception will be raised.

#### Fields

| name |  type | description |
|------|-------|-------------|
| LHS | [ComplexRangeExpression](./base_types.md#complexrangeexpression) | Range on the left-hand side, e.g. a measured time-trace. |
| RHS | [ComplexRangeExpression](./base_types.md#complexrangeexpression) | Range on the right-hand side, e.g. a weights array for demodulation. |


### ComplexConditional

A complex conditional expression.

**Inherits from:** [`ComplexExpression`](./base_types.md#complexexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Guard | [BooleanExpression](./base_types.md#booleanexpression) | Determines the case of the conditional. |
| TrueCase | [ComplexExpression](./base_types.md#complexexpression) | Output if the guard is true. |
| FalseCase | [ComplexExpression](./base_types.md#complexexpression) | Output if the guard is false. |


### BinaryComplexOperation

A binary operation between two complex expressions.

**Inherits from:** [`ComplexExpression`](./base_types.md#complexexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Operator | [BinaryComplexOperators](#binarycomplexoperators) | Operation to perform. |
| Lhs | [ComplexExpression](./base_types.md#complexexpression) | Left-hand side of the expression. |
| Rhs | [ComplexExpression](./base_types.md#complexexpression) | Right-hand side of the expression. |

### BinaryComplexOperators

An enumeration for binary complex operators.

* `Add` — addition between two complex expressions.
* `Subtract` — subtraction between two complex expressions.
* `Multiply` — multiplication between two complex expressions.

---

## Boolean Expressions

### ComparisonOperation

A boolean expression that defines a comparison between two numeric values.

**Inherits from:** [`BooleanExpression`](./base_types.md#booleanexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Operator | [ComparisonOperators](#comparisonoperators) | Comparison operation to perform. |
| LHS | [NumericExpression](./base_types.md#numericexpression) | Left-hand side of the comparison. |
| RHS | [NumericExpression](./base_types.md#numericexpression) | Right-hand side of the comparison. |

### ComparisonOperators

An enumeration for comparison operators.

* `LessThan` — $a < b$
* `GreaterThan` — $a > b$


### BooleanConditional

A boolean conditional expression.

**Inherits from:** [`BooleanExpression`](./base_types.md#booleanexpression)

#### Fields

| name |  type | description |
|------|-------|-------------|
| Guard | [BooleanExpression](./base_types.md#booleanexpression) | Determines the case of the conditional. |
| TrueCase | [BooleanExpression](./base_types.md#booleanexpression) | Output if the guard is true. |
| FalseCase | [BooleanExpression](./base_types.md#booleanexpression) | Output if the guard is false. |
