# Active Qubit Reset
This example demonstrates active qubit reset, in which a qubit is first readout and then a pi-pulse is applied conditionally on whether the qubit was found in the |1> state. The `BlockConditional` instruction provides the conditional branching.

### Example schedule
![Pulse schedule](./images/active_reset_schedule.svg)

### Tree format:
```mermaid
graph TD
    NumericExpression1["200"]
    Port1["Port"]
    Port1 -- "Id" --- NumericExpression1
    Frequency1["7 GHz"]
    Radians2["0 rad"]
    Frequency3["20 MHz"]
    Frame1["Frame"]
    Frame1 -- "Port" --- Port1
    Frame1 -- "Frequency" --- Frequency1
    Frame1 -- "Phase" --- Radians2
    Frame1 -- "IntermediateFrequency" --- Frequency3
    Interval4["1 μs"]
    Waveform1["ConstantWaveform"]
    Waveform1 -- "Duration" --- Interval4
    Instruction3["ModulatedPulse"]
    Instruction3 -- "Frame" --- Frame1
    Instruction3 -- "Envelope" --- Waveform1
    NumericExpression2["0.2"]
    Amplitude5["Amplitude"]
    Amplitude5 -- "Value" --- NumericExpression2
    Instruction3 -- "Amplitude" --- Amplitude5
    NumericExpression3["300"]
    Port2["Port"]
    Port2 -- "Id" --- NumericExpression3
    Interval6["1 μs"]
    ComplexRangeExpression1["AcquisitionComplexRangeResult"]
    Instruction4["AdcAcquisition"]
    Instruction4 -- "Port" --- Port2
    Instruction4 -- "Duration" --- Interval6
    Instruction4 -- "Result" --- ComplexRangeExpression1
    Instruction5:Alignment["StartToStart"]
    Instruction5:Relationship["InstructionRelationship"]
    Instruction5:Relationship -- "Alignment" --- Instruction5:Alignment
    Instruction5["Dependency"]
    Instruction5 -- "Relationship" --- Instruction5:Relationship
    Instruction5 -- "Lhs" --- Instruction3
    Instruction5 -- "Rhs" --- Instruction4
    ComplexRangeExpression2["Demodulation"]
    ComplexRangeExpression2 -- "Frame" --- Frame1
    ComplexRangeExpression2 -- "Trace" --- ComplexRangeExpression1
    ComplexRangeExpression3:Values["(0, 0), (1, 2), (3, 4)"]
    ComplexRangeExpression3["LiteralComplexRange"]
    ComplexRangeExpression3 -- "Values" --- ComplexRangeExpression3:Values
    ComplexExpression1["ComplexDotProduct"]
    ComplexExpression1 -- "Lhs" --- ComplexRangeExpression2
    ComplexExpression1 -- "Rhs" --- ComplexRangeExpression3
    NumericExpression4["ComplexRealValue"]
    NumericExpression4 -- "Operand" --- ComplexExpression1
    NumericExpression5["5"]
    BooleanExpression1["GreaterThan"]
    BooleanExpression1 -- "Lhs" --- NumericExpression4
    BooleanExpression1 -- "Rhs" --- NumericExpression5
    NumericExpression6["100"]
    Port3["Port"]
    Port3 -- "Id" --- NumericExpression6
    Frequency7["5 GHz"]
    Radians8["0 rad"]
    Frequency9["10 MHz"]
    Frame2["Frame"]
    Frame2 -- "Port" --- Port3
    Frame2 -- "Frequency" --- Frequency7
    Frame2 -- "Phase" --- Radians8
    Frame2 -- "IntermediateFrequency" --- Frequency9
    Interval10["100 ns"]
    Waveform2["ConstantWaveform"]
    Waveform2 -- "Duration" --- Interval10
    Instruction6["ModulatedPulse"]
    Instruction6 -- "Frame" --- Frame2
    Instruction6 -- "Envelope" --- Waveform2
    NumericExpression7["0.5"]
    Amplitude11["Amplitude"]
    Amplitude11 -- "Value" --- NumericExpression7
    Instruction6 -- "Amplitude" --- Amplitude11
    Interval12["100 ns"]
    Instruction7["Delay"]
    Instruction7 -- "Duration" --- Interval12
    Instruction8["BlockConditional"]
    Instruction8 -- "Guard" --- BooleanExpression1
    Instruction8 -- "TrueCase1" --- Instruction6
    Instruction8 -- "FalseCase1" --- Instruction7
    Instruction9:Alignment["EndToStart"]
    Instruction9:Relationship["InstructionRelationship"]
    Instruction9:Relationship -- "Alignment" --- Instruction9:Alignment
    Instruction9["Dependency"]
    Instruction9 -- "Relationship" --- Instruction9:Relationship
    Instruction9 -- "Lhs" --- Instruction5
    Instruction9 -- "Rhs" --- Instruction8
    Job["Job"]
    Job -- "EntryPoint1" --- Instruction9
```

### JSON format:
<details>
<summary>Job definition</summary>

``` JSON
{
    "version": "0.1.0",
    "compatible_version": "0.1.0",
    "acquisition_complex_range_results": {
        "AcquisitionComplexRangeResult1": {}
    },
    "frames": {
        "Frame1": {
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
        }
    },
    "entry_point": [
        {
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
                        "$type": "ConstantWaveform",
                        "duration": {
                            "$type": "NumericLiteral",
                            "value": 1E-06
                        }
                    },
                    "phase_offset": {
                        "$type": "NumericLiteral",
                        "value": 0
                    },
                    "amplitude": {
                        "$type": "NumericLiteral",
                        "value": 0.2
                    }
                },
                "rhs": {
                    "$type": "AdcAcquisition",
                    "port": {
                        "id": {
                            "$type": "NumericLiteral",
                            "value": 300
                        }
                    },
                    "duration": {
                        "$type": "NumericLiteral",
                        "value": 1E-06
                    },
                    "result": {
                        "$ref": "AcquisitionComplexRangeResult1"
                    }
                }
            },
            "rhs": {
                "$type": "BlockConditional",
                "guard": {
                    "$type": "ComparisonOperation",
                    "operator": "GreaterThan",
                    "lhs": {
                        "$type": "ComplexRealValue",
                        "operand": {
                            "$type": "ComplexDotProduct",
                            "lhs": {
                                "$type": "Demodulation",
                                "frame": {
                                    "$ref": "Frame1"
                                },
                                "trace": {
                                    "$ref": "AcquisitionComplexRangeResult1"
                                }
                            },
                            "rhs": {
                                "$type": "LiteralComplexRange",
                                "values": [
                                    [
                                        0,
                                        0
                                    ],
                                    [
                                        1,
                                        2
                                    ],
                                    [
                                        3,
                                        4
                                    ]
                                ]
                            }
                        }
                    },
                    "rhs": {
                        "$type": "NumericLiteral",
                        "value": 5
                    }
                },
                "true_case": [
                    {
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
                            "$type": "NumericLiteral",
                            "value": 0.5
                        }
                    }
                ],
                "false_case": [
                    {
                        "$type": "Delay",
                        "duration": {
                            "$type": "NumericLiteral",
                            "value": 1E-07
                        }
                    }
                ]
            }
        }
    ]
}
```
</details>
