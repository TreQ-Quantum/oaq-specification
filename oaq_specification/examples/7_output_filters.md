# Using Output Filters

This example shows how IIR and FIR output filters can be applied to ports, for e.g. flux pulse distortion correction. In particular, two IIR filters and an FIR filter are applied to Port 100 with the `FilterOutput` Instruction.

### Tree format:
```mermaid
graph TD
    NumericExpression1["100"]
    Port1["Port"]
    Port1 -- "Id" --- NumericExpression1
    Interval1["50 ns"]
    Interval2["10 ns"]
    Waveform1["GaussianWaveform"]
    Waveform1 -- "Duration" --- Interval1
    Waveform1 -- "Sigma" --- Interval2
    NumericExpression2["0.5"]
    Amplitude3["Amplitude"]
    Amplitude3 -- "Value" --- NumericExpression2
    Instruction1["UnmodulatedPulse"]
    Instruction1 -- "Port" --- Port1
    Instruction1 -- "Envelope" --- Waveform1
    Instruction1 -- "Amplitude" --- Amplitude3
    NumericExpression3["0.01"]
    Amplitude4["Amplitude"]
    Amplitude4 -- "Value" --- NumericExpression3
    Interval5["1 μs"]
    IirFilter6["IirFilter"]
    IirFilter6 -- "Amplitude" --- Amplitude4
    IirFilter6 -- "TimeConstant" --- Interval5
    NumericExpression4["0.001"]
    Amplitude7["Amplitude"]
    Amplitude7 -- "Value" --- NumericExpression4
    Interval8["500 ns"]
    IirFilter9["IirFilter"]
    IirFilter9 -- "Amplitude" --- Amplitude7
    IirFilter9 -- "TimeConstant" --- Interval8
    NumericRangeExpression1:Values["0.1, 0.15, 0.5, ..., 0.1"]
    NumericRangeExpression1["LiteralNumericRange"]
    NumericRangeExpression1 -- "Values" --- NumericRangeExpression1:Values
    FirFilter10["FirFilter"]
    FirFilter10 -- "Taps" --- NumericRangeExpression1
    Instruction2["FilterOutput"]
    Instruction2 -- "Port" --- Port1
    Instruction2 -- "Scope1" --- Instruction1
    Instruction2 -- "IirFilters1" --- IirFilter6
    Instruction2 -- "IirFilters2" --- IirFilter9
    Instruction2 -- "FirFilter" --- FirFilter10
    Job["Job"]
    Job -- "EntryPoint1" --- Instruction2
```

### JSON format:
<details>
<summary>Job definition</summary>

``` JSON
{
    "version": "0.1.0",
    "compatible_version": "0.1.0",
    "ports": {
        "Port1": {
            "id": {
                "$type": "NumericLiteral",
                "value": 100
            }
        }
    },
    "entry_point": [
        {
            "$type": "FilterOutput",
            "port": {
                "$ref": "Port1"
            },
            "scope": [
                {
                    "$type": "UnmodulatedPulse",
                    "port": {
                        "$ref": "Port1"
                    },
                    "envelope": {
                        "$type": "GaussianWaveform",
                        "duration": {
                            "$type": "NumericLiteral",
                            "value": 5E-08
                        },
                        "sigma": {
                            "$type": "NumericLiteral",
                            "value": 1E-08
                        }
                    },
                    "amplitude": {
                        "$type": "NumericLiteral",
                        "value": 0.5
                    }
                }
            ],
            "iir_filters": [
                {
                    "amplitude": {
                        "$type": "NumericLiteral",
                        "value": 0.01
                    },
                    "time_constant": {
                        "$type": "NumericLiteral",
                        "value": 1E-06
                    }
                },
                {
                    "amplitude": {
                        "$type": "NumericLiteral",
                        "value": 0.001
                    },
                    "time_constant": {
                        "$type": "NumericLiteral",
                        "value": 5E-07
                    }
                }
            ],
            "fir_filter": {
                "taps": {
                    "values": [
                        0.1,
                        0.15,
                        0.5,
                        0.15,
                        0.1
                    ]
                }
            }
        }
    ]
}
```
</details>
