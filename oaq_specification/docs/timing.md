# Timing

## Alignment

An enumeration that specifies which edges of two blocks should be aligned.

* `EndToStart` — for sequential dependencies: the right edge of the LHS block is aligned with the left edge of the RHS block.

* `StartToStart` — for parallel dependencies: the left edge of the LHS block is aligned with the left edge of the RHS block.

## InstructionRelationship

Defines an ordering relationship between a left-hand and right-hand block.

The user can specify the type of alignment as well as the minimum and maximum allowed offset that the instruction edges can differ by.

If strict timing is required, the min and max values should be set to confine the allowed offsets between the two blocks. A null value indicates no constraint on the minimum or maximum offset.

### Fields

| name |  type | description |
|------|-------|-------------|
| Alignment | [Alignment](#alignment) | The type of alignment to specify between the two blocks. |
| MinOffset | [Interval](./types.md#interval)? | The minimum allowed offset between the two aligned edges. Can be non-zero to indicate an intentional time displacement. |
| MaxOffset | [Interval](./types.md#interval)? | The maximum allowed offset between the two aligned edges. |

## Dependency

Specifies a timing dependency between two blocks.

**Inherits from:** [`Instruction`](./base_types.md#instruction)

The user can specify an instruction relationship between an ordered pair (LHS and RHS) of blocks. A backend will endeavour to schedule blocks as early as possible within the constraints of all dependencies, and will return an error if no valid resolution is found.

### Fields

| name |  type | description |
|------|-------|-------------|
| Relationship | [InstructionRelationship](#instructionrelationship) | The relationship to apply between the two blocks. |
| LHS | [Block](./block.md#block) | The block that relates to the left-hand side of the alignment. |
| RHS | [Block](./block.md#block) | The block that relates to the right-hand side of the alignment. |
