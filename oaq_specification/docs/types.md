# Types

## Amplitude
Represents a real-valued amplitude in arbitrary units.

### Fields

| name |  type | description |
|------|-------|-------------|
| Value | [NumericExpression](./base_types.md#numericexpression) | Value in arbitrary units

## Complex
Represents a complex-value.

For now, this can only be realized as an immutable literal.

### Fields

| name |  type | description |
|------|-------|-------------|
| Real | double | Real part of the complex number.
| Imaginary | double | Imaginary part of the complex number.

## Frequency
Represents a frequency value.

### Fields

| name |  type | description |
|------|-------|-------------|
| Hertz | [NumericExpression](./base_types.md#numericexpression) | Value in Hertz.

## Angle
Represents an angle value,

### Fields

| name |  type | description |
|------|-------|-------------|
| Radians | [NumericExpression](./base_types.md#numericexpression) | Value in Radians.

## Interval
Represents a duration in time.

### Fields

| name |  type | description |
|------|-------|-------------|
| Seconds | [NumericExpression](./base_types.md#numericexpression) | Value in seconds.
