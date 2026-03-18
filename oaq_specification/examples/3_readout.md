# A readout operation
In this example we define a readout operation consisting of a $1 \mu s$ constant pulse played on the readout drive port "200" and an ADC acquisition played on the acquisition port "100" in parallel for the same duration. The trace captured by the acquisition goes through the following post processing:

* Demodulation via an explicit Demodulation expression with a specified Frame.
* Integration via a ComplexDotProduct with a set of complex weights.
* Classification with a Comparison operator, setting to true if the demodulated value is greater than 5 and false otherwise. 

The classified value is then appended to a BooleanRangeRegister, and finally, the Register is specified as one of the job outputs with the name "classified_values" so that it will be returned to the user on job completion.

Note the matched min and max offsets specified for the drive/acquisition instruction relationship to ensure that the backend plays them at exactly the same time.

Below we also include the example JSON Results object that may be returned by running this Job.

### Example schedule
![Pulse schedule](./images/readout_schedule.svg)


### Tree format:
``` mermaid
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
    NumericExpression3["100"]
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
    BooleanRangeRegister1["BooleanRangeRegister"]
    Instruction6["BooleanAppend"]
    Instruction6 -- "Input" --- BooleanExpression1
    Instruction6 -- "Output" --- BooleanRangeRegister1
    Instruction7:Alignment["EndToStart"]
    Instruction7:Relationship["InstructionRelationship"]
    Instruction7:Relationship -- "Alignment" --- Instruction7:Alignment
    Instruction7["Dependency"]
    Instruction7 -- "Relationship" --- Instruction7:Relationship
    Instruction7 -- "Lhs" --- Instruction5
    Instruction7 -- "Rhs" --- Instruction6
    Job:BooleanRangeOutputs["BooleanRangeOutputs"]
    Job:BooleanRangeOutputs -- "0" --- BooleanRangeRegister1
    Job["Job"]
    Job -- "EntryPoint1" --- Instruction7
    Job -- "BooleanRangeOutputs" --- Job:BooleanRangeOutputs
```

### JSON format:
<details>
<summary>Job definition</summary>

``` JSON
{
    "version": "0.1.0",
    "compatible_version": "0.1.0",
    "boolean_range_registers": {
        "BooleanRangeRegister1": {
            "output_name": "classified_values"
        }
    },
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
                            "value": 100
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
                "$type": "BooleanAppend",
                "input": {
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
                "output": {
                    "$ref": "BooleanRangeRegister1"
                }
            }
        }
    ]
}
```
</details>

<details>
<summary>Example results</summary>

``` JSON

{
    "version": "0.1.0",
    "compatible_version": "0.1.0",
    "boolean_range_results": [
        {
            "name": "classified_values",
            "value": [false, true, true, false, ...]
        }
    ],
}
```
</details>
