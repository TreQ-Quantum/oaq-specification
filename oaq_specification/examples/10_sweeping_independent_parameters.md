# Sweeping Parameters in the Same Loop
This example demonstrates the sweeping of two different parameters within the loop. In this case, the amplitudes of two parallel pulses are swept across independent `LiteralNumericRanges` in the same ForEach body. The ForEach sweeps over an index, and two `NumericRangeElement` expressions are used to get the values from the ranges at these indexes.

### Tree format:
```mermaid
graph TD
    NumericExpression1["0"]
    NumericExpression2["1"]
    NumericExpression3["5"]
    NumericRange1["SteppedRange"]
    NumericRange1 -- "InclusiveStart" --- NumericExpression1
    NumericRange1 -- "Step" --- NumericExpression2
    NumericRange1 -- "ExclusiveEnd" --- NumericExpression3
    NumericParameter1["NumericParameter"]
    NumericExpression4["100"]
    Port1["Port"]
    Port1 -- "Id" --- NumericExpression4
    Frequency1["5 GHz"]
    Radians2["0 rad"]
    Frequency3["10 MHz"]
    Frame1["Frame"]
    Frame1 -- "Port" --- Port1
    Frame1 -- "Frequency" --- Frequency1
    Frame1 -- "Phase" --- Radians2
    Frame1 -- "IntermediateFrequency" --- Frequency3
    Interval4["100 ns"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval4
    Instruction3["ModulatedPulse"]
    Instruction3 -- "Frame" --- Frame1
    Instruction3 -- "Envelope" --- Waveform1
    NumericRangeExpression2:Values["0.1, 0.2, 0.3, ..., 0.5"]
    NumericRangeExpression2["LiteralNumericRange"]
    NumericRangeExpression2 -- "Values" --- NumericRangeExpression2:Values
    NumericExpression5["NumericRangeElement"]
    NumericExpression5 -- "Source" --- NumericRangeExpression2
    NumericExpression5 -- "Index" --- NumericParameter1
    Amplitude5["Amplitude"]
    Amplitude5 -- "Value" --- NumericExpression5
    Instruction3 -- "Amplitude" --- Amplitude5
    NumericExpression6["200"]
    Port2["Port"]
    Port2 -- "Id" --- NumericExpression6
    Frequency6["7 GHz"]
    Radians7["0 rad"]
    Frequency8["20 MHz"]
    Frame2["Frame"]
    Frame2 -- "Port" --- Port2
    Frame2 -- "Frequency" --- Frequency6
    Frame2 -- "Phase" --- Radians7
    Frame2 -- "IntermediateFrequency" --- Frequency8
    Interval9["100 ns"]
    Waveform2["ConstantWaveform"]
    Waveform2 -- "Duration" --- Interval9
    Instruction4["ModulatedPulse"]
    Instruction4 -- "Frame" --- Frame2
    Instruction4 -- "Envelope" --- Waveform2
    NumericRangeExpression3:Values["0.5, 0.4, 0.3, ..., 0.1"]
    NumericRangeExpression3["LiteralNumericRange"]
    NumericRangeExpression3 -- "Values" --- NumericRangeExpression3:Values
    NumericExpression7["NumericRangeElement"]
    NumericExpression7 -- "Source" --- NumericRangeExpression3
    NumericExpression7 -- "Index" --- NumericParameter1
    Amplitude10["Amplitude"]
    Amplitude10 -- "Value" --- NumericExpression7
    Instruction4 -- "Amplitude" --- Amplitude10
    Instruction5:Alignment["StartToStart"]
    Instruction5:Relationship["InstructionRelationship"]
    Instruction5:Relationship -- "Alignment" --- Instruction5:Alignment
    Instruction5["Dependency"]
    Instruction5 -- "Relationship" --- Instruction5:Relationship
    Instruction5 -- "Lhs" --- Instruction3
    Instruction5 -- "Rhs" --- Instruction4
    Instruction6:Alignment["EndToStart"]
    Instruction6:Relationship["InstructionRelationship"]
    Instruction6:Relationship -- "Alignment" --- Instruction6:Alignment
    Function7["Function"]
    Function7 -- "Parameter" --- NumericParameter1
    Function7 -- "Body1" --- Instruction5
    Instruction6["ForEachNumeric"]
    Instruction6 -- "Source" --- NumericRange1
    Instruction6 -- "Relationship" --- Instruction6:Relationship
    Instruction6 -- "Body" --- Function7
    Job["Job"]
    Job -- "EntryPoint1" --- Instruction6
```

### JSON format:
<details>
<summary>Job definition</summary>

``` JSON
{
    "version": "0.1.0",
    "compatible_version": "0.1.0",
    "numeric_parameters": {
        "NumericParameter1": {}
    },
    "entry_point": [
        {
            "$type": "ForEachNumeric",
            "source": {
                "$type": "SteppedRange",
                "inclusive_start": {
                    "$type": "NumericLiteral",
                    "value": 0
                },
                "step": {
                    "$type": "NumericLiteral",
                    "value": 1
                },
                "exclusive_end": {
                    "$type": "NumericLiteral",
                    "value": 5
                }
            },
            "relationship": {},
            "body_name": null,
            "parameter": {
                "$ref": "NumericParameter1"
            },
            "body_content": [
                {
                    "$type": "Dependency",
                    "relationship": {
                        "alignment": "StartToStart"
                    },
                    "lhs": {
                        "$type": "ModulatedPulse",
                        "frame": {
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
                        },
                        "envelope": {
                            "$type": "ConstantWaveform",
                            "duration": {
                                "$type": "NumericLiteral",
                                "value": 1E-07
                            }
                        },
                        "phase_offset": {
                            "$type": "NumericLiteral",
                            "value": 0
                        },
                        "amplitude": {
                            "$type": "NumericRangeElement",
                            "source": {
                                "$type": "LiteralNumericRange",
                                "values": [
                                    0.1,
                                    0.2,
                                    0.3,
                                    0.4,
                                    0.5
                                ]
                            },
                            "index": {
                                "$type": "NumericParameter",
                                "$ref": "NumericParameter1"
                            }
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
                            "$type": "ConstantWaveform",
                            "duration": {
                                "$type": "NumericLiteral",
                                "value": 1E-07
                            }
                        },
                        "phase_offset": {
                            "$type": "NumericLiteral",
                            "value": 0
                        },
                        "amplitude": {
                            "$type": "NumericRangeElement",
                            "source": {
                                "$type": "LiteralNumericRange",
                                "values": [
                                    0.5,
                                    0.4,
                                    0.3,
                                    0.2,
                                    0.1
                                ]
                            },
                            "index": {
                                "$type": "NumericParameter",
                                "$ref": "NumericParameter1"
                            }
                        }
                    }
                }
            ]
        }
    ]
}
```
</details>
