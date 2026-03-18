## Dependencies and dependency chains
When defining jobs with more than one instruction, dependencies must be specified to declare how one instruction should depend on another in time.

The simplest example is two instructions, the second of which starts when the first one completes.
(A | B)

``` mermaid
graph TD
    Frame1["Frame"]
    Interval1["50 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Instruction2["ModulatedPulse"]
    Instruction2 -- "Frame" --- Frame1
    Instruction2 -- "Envelope" --- Waveform1
    Radians2["1 rad"]
    Instruction2 -- "PhaseOffset" --- Radians2
    Instruction3["ModulatedPulse"]
    Instruction3 -- "Frame" --- Frame1
    Instruction3 -- "Envelope" --- Waveform1
    Radians3["2 rad"]
    Instruction3 -- "PhaseOffset" --- Radians3
    Instruction4:Alignment["EndToStart"]
    Instruction4:Relationship["InstructionRelationship"]
    Instruction4:Relationship -- "Alignment" --- Instruction4:Alignment
    Instruction4["Dependency"]
    Instruction4 -- "Relationship" --- Instruction4:Relationship
    Instruction4 -- "Lhs" --- Instruction2
    Instruction4 -- "Rhs" --- Instruction3
```

Because, like drive instructions, Dependencies inherit from Instruction, they themselves can be fields in another dependency, i.e. one can declare a dependency between two other dependencies or a dependency and another instruction. This enables three instructions to be chained.
(A | B | C)

``` mermaid
graph TD
    Frame1["Frame"]
    Interval1["50 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Instruction3["ModulatedPulse"]
    Instruction3 -- "Frame" --- Frame1
    Instruction3 -- "Envelope" --- Waveform1
    Radians2["1 rad"]
    Instruction3 -- "PhaseOffset" --- Radians2
    Instruction4["ModulatedPulse"]
    Instruction4 -- "Frame" --- Frame1
    Instruction4 -- "Envelope" --- Waveform1
    Radians3["2 rad"]
    Instruction4 -- "PhaseOffset" --- Radians3
    Instruction5:Alignment["EndToStart"]
    Instruction5:Relationship["InstructionRelationship"]
    Instruction5:Relationship -- "Alignment" --- Instruction5:Alignment
    Instruction5["Dependency"]
    Instruction5 -- "Relationship" --- Instruction5:Relationship
    Instruction5 -- "Lhs" --- Instruction3
    Instruction5 -- "Rhs" --- Instruction4
    Instruction6["ModulatedPulse"]
    Instruction6 -- "Frame" --- Frame1
    Instruction6 -- "Envelope" --- Waveform1
    Radians4["3 rad"]
    Instruction6 -- "PhaseOffset" --- Radians4
    Instruction7:Alignment["EndToStart"]
    Instruction7:Relationship["InstructionRelationship"]
    Instruction7:Relationship -- "Alignment" --- Instruction7:Alignment
    Instruction7["Dependency"]
    Instruction7 -- "Relationship" --- Instruction7:Relationship
    Instruction7 -- "Lhs" --- Instruction5
    Instruction7 -- "Rhs" --- Instruction6
```

While more complex arrangements are possible, the normal way to create sequences of multiple instructions is to create a left-heavy tree. An unbalanced tree may seem inefficient to someone unfamiliar with ASTs but is actually normal.
(A | B | C | D)

``` mermaid
graph TD
    Frame1["Frame"]
    Interval1["50 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Instruction4["ModulatedPulse"]
    Instruction4 -- "Frame" --- Frame1
    Instruction4 -- "Envelope" --- Waveform1
    Radians2["1 rad"]
    Instruction4 -- "PhaseOffset" --- Radians2
    Instruction5["ModulatedPulse"]
    Instruction5 -- "Frame" --- Frame1
    Instruction5 -- "Envelope" --- Waveform1
    Radians3["2 rad"]
    Instruction5 -- "PhaseOffset" --- Radians3
    Instruction6:Alignment["EndToStart"]
    Instruction6:Relationship["InstructionRelationship"]
    Instruction6:Relationship -- "Alignment" --- Instruction6:Alignment
    Instruction6["Dependency"]
    Instruction6 -- "Relationship" --- Instruction6:Relationship
    Instruction6 -- "Lhs" --- Instruction4
    Instruction6 -- "Rhs" --- Instruction5
    Instruction7["ModulatedPulse"]
    Instruction7 -- "Frame" --- Frame1
    Instruction7 -- "Envelope" --- Waveform1
    Radians4["3 rad"]
    Instruction7 -- "PhaseOffset" --- Radians4
    Instruction8:Alignment["EndToStart"]
    Instruction8:Relationship["InstructionRelationship"]
    Instruction8:Relationship -- "Alignment" --- Instruction8:Alignment
    Instruction8["Dependency"]
    Instruction8 -- "Relationship" --- Instruction8:Relationship
    Instruction8 -- "Lhs" --- Instruction6
    Instruction8 -- "Rhs" --- Instruction7
    Instruction9["ModulatedPulse"]
    Instruction9 -- "Frame" --- Frame1
    Instruction9 -- "Envelope" --- Waveform1
    Radians5["4 rad"]
    Instruction9 -- "PhaseOffset" --- Radians5
    Instruction10:Alignment["EndToStart"]
    Instruction10:Relationship["InstructionRelationship"]
    Instruction10:Relationship -- "Alignment" --- Instruction10:Alignment
    Instruction10["Dependency"]
    Instruction10 -- "Relationship" --- Instruction10:Relationship
    Instruction10 -- "Lhs" --- Instruction8
    Instruction10 -- "Rhs" --- Instruction9
```

All the above examples use a end-start pulse edge alignment where one instruction sequentially follows the other. Currently, a user can specify either this end-start pulse edge alignment or a start-start alignment where two pulses are played in parallel.
(A / B)

``` mermaid
graph TD
    Frame1["Frame"]
    Interval1["50 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Instruction2["ModulatedPulse"]
    Instruction2 -- "Frame" --- Frame1
    Instruction2 -- "Envelope" --- Waveform1
    Radians2["1 rad"]
    Instruction2 -- "PhaseOffset" --- Radians2
    Instruction3["ModulatedPulse"]
    Instruction3 -- "Frame" --- Frame1
    Instruction3 -- "Envelope" --- Waveform1
    Radians3["2 rad"]
    Instruction3 -- "PhaseOffset" --- Radians3
    Instruction4:Alignment["StartToStart"]
    Instruction4:Relationship["InstructionRelationship"]
    Instruction4:Relationship -- "Alignment" --- Instruction4:Alignment
    Instruction4["Dependency"]
    Instruction4 -- "Relationship" --- Instruction4:Relationship
    Instruction4 -- "Lhs" --- Instruction2
    Instruction4 -- "Rhs" --- Instruction3
```

Similarly to sequential chains, multiple instructions that start at the same time tend to be represented by left-heavy trees.
(A / B / C)

``` mermaid
graph TD
    Frame1["Frame"]
    Interval1["50 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Instruction3["ModulatedPulse"]
    Instruction3 -- "Frame" --- Frame1
    Instruction3 -- "Envelope" --- Waveform1
    Radians2["1 rad"]
    Instruction3 -- "PhaseOffset" --- Radians2
    Instruction4["ModulatedPulse"]
    Instruction4 -- "Frame" --- Frame1
    Instruction4 -- "Envelope" --- Waveform1
    Radians3["2 rad"]
    Instruction4 -- "PhaseOffset" --- Radians3
    Instruction5:Alignment["StartToStart"]
    Instruction5:Relationship["InstructionRelationship"]
    Instruction5:Relationship -- "Alignment" --- Instruction5:Alignment
    Instruction5["Dependency"]
    Instruction5 -- "Relationship" --- Instruction5:Relationship
    Instruction5 -- "Lhs" --- Instruction3
    Instruction5 -- "Rhs" --- Instruction4
    Instruction6["ModulatedPulse"]
    Instruction6 -- "Frame" --- Frame1
    Instruction6 -- "Envelope" --- Waveform1
    Radians4["3 rad"]
    Instruction6 -- "PhaseOffset" --- Radians4
    Instruction7:Alignment["StartToStart"]
    Instruction7:Relationship["InstructionRelationship"]
    Instruction7:Relationship -- "Alignment" --- Instruction7:Alignment
    Instruction7["Dependency"]
    Instruction7 -- "Relationship" --- Instruction7:Relationship
    Instruction7 -- "Lhs" --- Instruction5
    Instruction7 -- "Rhs" --- Instruction6
```

Barrier instructions in other paradigms would be represented by an end-to-start dependency between trees of instructions linked by start-to-start dependencies as in the following example.
((A / B) | (C / D / E))

``` mermaid
graph TD
    Frame1["Frame"]
    Interval1["50 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Instruction3["ModulatedPulse"]
    Instruction3 -- "Frame" --- Frame1
    Instruction3 -- "Envelope" --- Waveform1
    Radians2["1 rad"]
    Instruction3 -- "PhaseOffset" --- Radians2
    Instruction4["ModulatedPulse"]
    Instruction4 -- "Frame" --- Frame1
    Instruction4 -- "Envelope" --- Waveform1
    Radians3["2 rad"]
    Instruction4 -- "PhaseOffset" --- Radians3
    Instruction5:Alignment["StartToStart"]
    Instruction5:Relationship["InstructionRelationship"]
    Instruction5:Relationship -- "Alignment" --- Instruction5:Alignment
    Instruction5["Dependency"]
    Instruction5 -- "Relationship" --- Instruction5:Relationship
    Instruction5 -- "Lhs" --- Instruction3
    Instruction5 -- "Rhs" --- Instruction4
    Instruction8["ModulatedPulse"]
    Instruction8 -- "Frame" --- Frame1
    Instruction8 -- "Envelope" --- Waveform1
    Radians4["3 rad"]
    Instruction8 -- "PhaseOffset" --- Radians4
    Instruction9["ModulatedPulse"]
    Instruction9 -- "Frame" --- Frame1
    Instruction9 -- "Envelope" --- Waveform1
    Radians5["4 rad"]
    Instruction9 -- "PhaseOffset" --- Radians5
    Instruction10:Alignment["StartToStart"]
    Instruction10:Relationship["InstructionRelationship"]
    Instruction10:Relationship -- "Alignment" --- Instruction10:Alignment
    Instruction10["Dependency"]
    Instruction10 -- "Relationship" --- Instruction10:Relationship
    Instruction10 -- "Lhs" --- Instruction8
    Instruction10 -- "Rhs" --- Instruction9
    Instruction11["ModulatedPulse"]
    Instruction11 -- "Frame" --- Frame1
    Instruction11 -- "Envelope" --- Waveform1
    Radians6["5 rad"]
    Instruction11 -- "PhaseOffset" --- Radians6
    Instruction12:Alignment["StartToStart"]
    Instruction12:Relationship["InstructionRelationship"]
    Instruction12:Relationship -- "Alignment" --- Instruction12:Alignment
    Instruction12["Dependency"]
    Instruction12 -- "Relationship" --- Instruction12:Relationship
    Instruction12 -- "Lhs" --- Instruction10
    Instruction12 -- "Rhs" --- Instruction11
    Instruction13:Alignment["EndToStart"]
    Instruction13:Relationship["InstructionRelationship"]
    Instruction13:Relationship -- "Alignment" --- Instruction13:Alignment
    Instruction13["Dependency"]
    Instruction13 -- "Relationship" --- Instruction13:Relationship
    Instruction13 -- "Lhs" --- Instruction5
    Instruction13 -- "Rhs" --- Instruction12
```

The next example is more complex, we realize a set of four instructions with order dependencies as shown by the graph below, including both EndToStart and StartToStart alignments.

![Dependency chain graph](./images/dependency_graph.png)

The example also flags another feature of the specification where some nodes can be children of more than one parent. In this case, instructions A, B & D use the same Frame, and instructions A & B use the same waveform (with a different amplitude).

### Example schedule
![Pulse schedule](./images/dependency_chain_schedule.svg)

### Tree format:
``` mermaid
graph TD
    Frame1["Frame"]
    Interval1["50 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Instruction2["ModulatedPulse"]
    Instruction2 -- "Frame" --- Frame1
    Instruction2 -- "Envelope" --- Waveform1
    Radians2["1 rad"]
    Instruction2 -- "PhaseOffset" --- Radians2
    Instruction5["ModulatedPulse"]
    Instruction5 -- "Frame" --- Frame1
    Instruction5 -- "Envelope" --- Waveform1
    Radians3["2 rad"]
    Instruction5 -- "PhaseOffset" --- Radians3
    Frame2["Frame"]
    Instruction6["ModulatedPulse"]
    Instruction6 -- "Frame" --- Frame2
    Instruction6 -- "Envelope" --- Waveform1
    Radians4["3 rad"]
    Instruction6 -- "PhaseOffset" --- Radians4
    Instruction7:Alignment["StartToStart"]
    Instruction7:Relationship["InstructionRelationship"]
    Instruction7:Relationship -- "Alignment" --- Instruction7:Alignment
    Instruction7["Dependency"]
    Instruction7 -- "Relationship" --- Instruction7:Relationship
    Instruction7 -- "Lhs" --- Instruction5
    Instruction7 -- "Rhs" --- Instruction6
    Instruction8["ModulatedPulse"]
    Instruction8 -- "Frame" --- Frame1
    Instruction8 -- "Envelope" --- Waveform1
    Radians5["4 rad"]
    Instruction8 -- "PhaseOffset" --- Radians5
    Instruction9:Alignment["EndToStart"]
    Instruction9:Relationship["InstructionRelationship"]
    Instruction9:Relationship -- "Alignment" --- Instruction9:Alignment
    Instruction9["Dependency"]
    Instruction9 -- "Relationship" --- Instruction9:Relationship
    Instruction9 -- "Lhs" --- Instruction7
    Instruction9 -- "Rhs" --- Instruction8
    Instruction10:Alignment["EndToStart"]
    Instruction10:Relationship["InstructionRelationship"]
    Instruction10:Relationship -- "Alignment" --- Instruction10:Alignment
    Instruction10["Dependency"]
    Instruction10 -- "Relationship" --- Instruction10:Relationship
    Instruction10 -- "Lhs" --- Instruction2
    Instruction10 -- "Rhs" --- Instruction9
```

### JSON format:
<details>
<summary>Job definition</summary>

``` JSON
{
    "version": "0.1.0",
    "compatible_version": "0.1.0",
    "frames": {
        "Frame1": {
            "port": {
                "id": {
                    "$type": "NumericLiteral",
                    "value": 100
                }
            },
            "frequency": {
                "$type": "NumericLiteral",
                "value": 5000000000
            },
            "phase": {
                "$type": "NumericLiteral",
                "value": 0
            },
            "intermediate_frequency": {
                "$type": "NumericLiteral",
                "value": 10000000
            }
        }
    },
    "waveforms": {
        "Waveform1": {
            "$type": "ConstantWaveform",
            "duration": {
                "$type": "NumericLiteral",
                "value": 5E-08
            }
        }
    },
    "entry_point": [
        {
            "$type": "Dependency",
            "relationship": {},
            "lhs": {
                "$type": "ModulatedPulse",
                "frame": {
                    "$ref": "Frame1"
                },
                "envelope": {
                    "$ref": "Waveform1"
                },
                "phase_offset": {
                    "$type": "NumericLiteral",
                    "value": 1
                },
                "amplitude": {
                    "$type": "NumericLiteral",
                    "value": 1
                }
            },
            "rhs": {
                "$type": "Dependency",
                "relationship": {},
                "lhs": {
                    "$type": "Dependency",
                    "relationship": {
                        "alignment": "StartToStart"
                    },
                    "lhs": {
                        "$type": "ModulatedPulse",
                        "frame": {
                            "$ref": "Frame1"
                        },
                        "envelope": {
                            "$ref": "Waveform1"
                        },
                        "phase_offset": {
                            "$type": "NumericLiteral",
                            "value": 2
                        },
                        "amplitude": {
                            "$type": "NumericLiteral",
                            "value": 1
                        }
                    },
                    "rhs": {
                        "$type": "ModulatedPulse",
                        "frame": {
                            "port": {
                                "id": {
                                    "$type": "NumericLiteral",
                                    "value": 200
                                }
                            },
                            "frequency": {
                                "$type": "NumericLiteral",
                                "value": 7000000000
                            },
                            "phase": {
                                "$type": "NumericLiteral",
                                "value": 0
                            },
                            "intermediate_frequency": {
                                "$type": "NumericLiteral",
                                "value": 20000000
                            }
                        },
                        "envelope": {
                            "$ref": "Waveform1"
                        },
                        "phase_offset": {
                            "$type": "NumericLiteral",
                            "value": 3
                        },
                        "amplitude": {
                            "$type": "NumericLiteral",
                            "value": 1
                        }
                    }
                },
                "rhs": {
                    "$type": "ModulatedPulse",
                    "frame": {
                        "$ref": "Frame1"
                    },
                    "envelope": {
                        "$ref": "Waveform1"
                    },
                    "phase_offset": {
                        "$type": "NumericLiteral",
                        "value": 4
                    },
                    "amplitude": {
                        "$type": "NumericLiteral",
                        "value": 1
                    }
                }
            }
        }
    ]
}
```
</details>
