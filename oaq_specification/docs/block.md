## Block
Defines a list of Instruction-like roots that form the body of a program.

### Fields

| name |  type | description |
|------|-------|-------------|
| Instructions | [Instruction](./base_types.md#instruction)[] | A list of Instruction-like objects to use as roots of a dependency graph that makes up the body of the program. Valid objects can be Instructions, Dependencies, and ForEach.
