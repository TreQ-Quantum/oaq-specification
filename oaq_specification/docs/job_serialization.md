# JSON Input Format

A job to be run can be serialized as a JSON string and submitted to the backend.

## JSON Schema

The format is formalized below as a JSON Schema, and implementations can be validated against this.

Note that an input that validates against this schema may still not validate against the data model (e.g. providing a reference to something other than a waveform in a ModulatedPulse's "envelope" property), or the backend's constraints (e.g. an amplitude that exceeds a port's limits).

``` json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "properties": {
    "version": { "const": "0.1.0" },
    "compatible_version": { "const": "0.1.0" },
    "entry_point": { "$ref": "#/$defs/Block" },
    "ports": {
      "type": "object",
      "additionalProperties": { "$ref": "#/$defs/Port" }
    },
    "frames": {
      "type": "object",
      "additionalProperties": { "$ref": "#/$defs/Frame" }
    },
    "waveforms": {
      "type": "object",
      "additionalProperties": { "$ref": "#/$defs/Waveform" }
    },
    "instructions": {
      "type": "object",
      "additionalProperties": { "$ref": "#/$defs/Instruction" }
    },
    "numeric_parameters": {
      "type": "object",
      "additionalProperties": {
        "type": "object",
        "properties": {},
        "additionalProperties": false
      }
    },
    "boolean_range_registers": { "$ref": "#/$defs/RegisterDefinitions" },
    "numeric_range_registers": { "$ref": "#/$defs/RegisterDefinitions" },
    "complex_range_registers": { "$ref": "#/$defs/RegisterDefinitions" },
    "acquisition_complex_range_results": {
      "type": "object",
      "additionalProperties": { "$ref": "#/$defs/EmptyObject" }
    }
  },
  "required": [
    "version",
    "compatible_version",
    "entry_point"
  ],
  "additionalProperties": false,

  "$defs": {
    "Reference": {
      "type": "object",
      "properties": {
        "$ref": { "type": "string" }
      },
      "required": [ "$ref" ],
      "additionalProperties": false
    },
    "EmptyObject": {
      "type": "object",
      "properties": {},
      "required": [],
      "additionalProperties": false
    },
    "Complex": {
      "type": "array",
      "items": { "type": "number" },
      "minItems": 2,
      "maxItems": 2
    },
    "Block": {
      "type": "array",
      "items": { "$ref": "#/$defs/InstructionOrReference" }
    },
    "Port": {
      "type": "object",
      "properties": {
        "id": { "$ref": "#/$defs/NumericExpression" }
      },
      "required": [ "id" ],
      "additionalProperties": false
    },
    "PortOrReference": {
      "oneOf": [
        { "$ref": "#/$defs/Reference" },
        { "$ref": "#/$defs/Port" }
      ]
    },
    "IirFilter": {
      "type": "object",
      "properties": {
        "amplitude": { "$ref": "#/$defs/NumericExpression" },
        "time_constant": { "$ref": "#/$defs/NumericExpression" }
      },
      "required": [ "amplitude", "time_constant" ],
      "additionalProperties": false
    },
    "FirFilter": {
      "type": "object",
      "properties": {
        "taps": { "$ref": "#/$defs/LiteralNumericRange" },
      },
      "required": [ "taps"],
      "additionalProperties": false
    },
    "Frame": {
      "type": "object",
      "properties": {
        "port": { "$ref": "#/$defs/PortOrReference" },
        "frequency": { "$ref": "#/$defs/NumericExpression" },
        "phase": { "$ref": "#/$defs/NumericExpression" },
        "intermediate_frequency": { "$ref": "#/$defs/OptionalNumericExpression" }
      },
      "required": [ "port", "frequency", "phase", "intermediate_frequency" ],
      "additionalProperties": false
    },
    "FrameOrReference": {
      "oneOf": [
        { "$ref": "#/$defs/Reference" },
        { "$ref": "#/$defs/Frame" }
      ]
    },
    "Instruction": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "Dependency" },
            "relationship": { "$ref": "#/$defs/InstructionRelationship" },
            "lhs": { "$ref": "#/$defs/InstructionOrReference" },
            "rhs": { "$ref": "#/$defs/InstructionOrReference" }
          },
          "required": [ "$type", "relationship", "lhs", "rhs" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ForEachNumeric" },
            "source": { "$ref": "#/$defs/NumericRangeExpression" },
            "relationship": { "$ref": "#/$defs/InstructionRelationship" },
            "body_name": { "$ref": "#/$defs/OptionalString" },
            "body_content": { "$ref": "#/$defs/Block" },
            "parameter": { "$ref": "#/$defs/RegisterOrParameterOrReference" }
          },
          "required": [ "$type", "source", "relationship", "body_name", "body_content", "parameter" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "DcBias" },
            "port": { "$ref": "#/$defs/PortOrReference" },
            "amplitude": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "port", "amplitude" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "Delay" },
            "duration": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "duration" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ShiftFramePhase" },
            "frame": { "$ref": "#/$defs/FrameOrReference" },
            "additional_phase": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "frame", "additional_phase" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "SetFramePhase" },
            "frame": { "$ref": "#/$defs/FrameOrReference" },
            "phase": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "frame", "phase" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "DetuneFrame" },
            "frame": { "$ref": "#/$defs/FrameOrReference" },
            "detuning": { "$ref": "#/$defs/NumericExpression" },
            "scope": { "$ref": "#/$defs/Block" },
            "phase_reference": {
              "type": "string",
              "enum": [ "T0IsJobStart", "T0IsNow" ]
            },
          },
          "required": [ "$type", "frame", "detuning", "scope", "phase_reference" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "FilterOutput" },
            "port": { "$ref": "#/$defs/PortOrReference" },
            "scope": { "$ref": "#/$defs/Block" },
            "iir_filters": { "type": "array", "items": { "$ref": "#/$defs/IirFilter" }},
            "fir_filter": { "$ref": "#/$defs/FirFilter" },
          },
          "required": [ "$type", "port", "scope", "iir_filters", "fir_filter" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BlockConditional" },
            "guard": { "$ref": "#/$defs/BooleanExpression" },
            "true_case": { "$ref": "#/$defs/Block" },
            "false_case": { "$ref": "#/$defs/Block" }
          },
          "required": [ "$type", "guard", "true_case", "false_case" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "UnmodulatedPulse" },
            "port": { "$ref": "#/$defs/PortOrReference" },
            "envelope": { "$ref": "#/$defs/WaveformOrReference" },
            "amplitude": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "port", "envelope", "amplitude" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ModulatedPulse" },
            "frame": { "$ref": "#/$defs/FrameOrReference" },
            "envelope": { "$ref": "#/$defs/WaveformOrReference" },
            "amplitude": { "$ref": "#/$defs/NumericExpression" },
            "phase_offset": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "frame", "envelope", "amplitude", "phase_offset" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "AdcAcquisition" },
            "port": { "$ref": "#/$defs/PortOrReference" },
            "duration": { "$ref": "#/$defs/NumericExpression" },
            "result": { "$ref": "#/$defs/AcquisitionResultOrReference" }
          },
          "required": [ "$type", "port", "duration", "result" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BooleanAppend" },
            "input": { "$ref": "#/$defs/BooleanExpression" },
            "output": { "$ref": "#/$defs/RegisterOrParameterOrReference" }
          },
          "required": [ "$type", "input", "output" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericAppend" },
            "input": { "$ref": "#/$defs/NumericExpression" },
            "output": { "$ref": "#/$defs/RegisterOrParameterOrReference" }
          },
          "required": [ "$type", "input", "output" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexAppend" },
            "input": { "$ref": "#/$defs/ComplexExpression" },
            "output": { "$ref": "#/$defs/RegisterOrParameterOrReference" }
          },
          "required": [ "$type", "input", "output" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexRangeAssign" },
            "input": { "$ref": "#/$defs/ComplexRangeExpression" },
            "output": { "$ref": "#/$defs/RegisterOrParameterOrReference" }
          },
          "required": [ "$type", "input", "output" ],
          "additionalProperties": false
        }
      ]
    },
    "InstructionOrReference": {
      "oneOf": [
        { "$ref": "#/$defs/Reference" },
        { "$ref": "#/$defs/Instruction" }
      ]
    },
    "Waveform": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ConstantWaveform" },
            "duration": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "duration" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "CosineWaveform" },
            "duration": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "duration" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BlackmanWaveform" },
            "duration": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "duration" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "GaussianWaveform" },
            "duration": { "$ref": "#/$defs/NumericExpression" },
            "sigma": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "duration", "sigma" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "SampledNumericWaveform" },
            "sample_interval": { "$ref": "#/$defs/NumericExpression" },
            "samples": {
              "type": "array",
              "items": { "type": "number" }
            }
          },
          "required": [ "$type", "sample_interval", "samples" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "SampledComplexWaveform" },
            "sample_interval": { "$ref": "#/$defs/NumericExpression" },
            "samples": {
              "type": "array",
              "items": { "$ref": "#/$defs/Complex" }
            }
          },
          "required": [ "$type", "sample_interval", "samples" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "SuddenNetZeroWaveform" },
            "duration": { "$ref": "#/$defs/NumericExpression" },
            "midpoint_delay": { "$ref": "#/$defs/NumericExpression" },
            "b_amplitude": { "$ref": "#/$defs/NumericExpression" },
            "b_duration": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "duration", "midpoint_delay", "b_amplitude", "b_duration" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "PlateauWaveform" },
            "base": { "$ref": "#/$defs/WaveformOrReference" },
            "plateau_duration": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "base", "plateau_duration" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "DragWaveform" },
            "base": { "$ref": "#/$defs/WaveformOrReference" },
            "drag_parameter": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "base", "drag_parameter" ],
          "additionalProperties": false
        }
      ]
    },
    "WaveformOrReference": {
      "oneOf": [
        { "$ref": "#/$defs/Reference" },
        { "$ref": "#/$defs/Waveform" }
      ]
    },
    "InstructionRelationship": {
      "type": "object",
      "properties": {
        "alignment": {
          "type": "string",
          "enum": [ "StartToStart", "EndToStart" ]
        },
        "min_offset": { "$ref": "#/$defs/OptionalNumericExpression" },
        "max_offset": { "$ref": "#/$defs/OptionalNumericExpression" }
      },
      "required": [],
      "additionalProperties": false
    },
    "RegisterDefinitions": {
      "type": "object",
      "additionalProperties": {
        "type": "object",
        "properties": {
          "output_name": { "$ref": "#/$defs/OptionalString" },
        },
        "required": [ "output_name" ],
        "additionalProperties": false
      }
    },
    "RegisterOrParameterOrReference": {
      "oneOf": [
        { "$ref": "#/$defs/Reference" },
        {
          "type": "object",
          "properties": {
            "name": { "type": "string" }
          },
          "additionalProperties": false
        }
      ]
    },
    "AcquisitionResultOrReference": {
      "oneOf": [
        { "$ref": "#/$defs/Reference" },
        { "$ref": "#/$defs/EmptyObject" }
      ]
    },
    "NumericExpression": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericLiteral" },
            "value": { "type": "number" }
          },
          "required": [ "$type", "value" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericParameter" },
            "$ref": { "type": "string" }
          },
          "required": [ "$type", "$ref" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericParameter" },
          },
          "required": [ "$type" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexRealValue" },
            "operand": { "$ref": "#/$defs/ComplexExpression" }
          },
          "required": [ "$type", "operand" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexImaginaryValue" },
            "operand": { "$ref": "#/$defs/ComplexExpression" }
          },
          "required": [ "$type", "operand" ],
          "additionalProperties": false
        },
                {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericConditional" },
            "guard": { "$ref": "#/$defs/BooleanExpression" },
            "true_case": { "$ref": "#/$defs/NumericExpression" },
            "false_case": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "guard", "true_case", "false_case" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericRangeElement" },
            "index": { "$ref": "#/$defs/NumericExpression" },
            "source": { "$ref": "#/$defs/NumericRangeExpression" }
          },
          "required": [ "$type", "index", "source" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BinaryNumericOperation" },
            "operator": {
              "type": "string",
              "enum": [ "Add", "Subtract", "Multiply" ]
            },
            "lhs": { "$ref": "#/$defs/NumericExpression" },
            "rhs": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "operator", "lhs", "rhs" ],
          "additionalProperties": false
        }
      ]
    },
    "ComplexExpression": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexDotProduct" },
            "lhs": { "$ref": "#/$defs/ComplexRangeExpression" },
            "rhs": { "$ref": "#/$defs/ComplexRangeExpression" }
          },
          "required": [ "$type", "lhs", "rhs" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexConditional" },
            "guard": { "$ref": "#/$defs/BooleanExpression" },
            "true_case": { "$ref": "#/$defs/ComplexExpression" },
            "false_case": { "$ref": "#/$defs/ComplexExpression" }
          },
          "required": [ "$type", "guard", "true_case", "false_case" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexRangeElement" },
            "index": { "$ref": "#/$defs/NumericExpression" },
            "source": { "$ref": "#/$defs/ComplexRangeExpression" }
          },
          "required": [ "$type", "index", "source" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BinaryComplexOperation" },
            "operator": {
              "type": "string",
              "enum": [ "Add", "Subtract", "Multiply" ]
            },
            "lhs": { "$ref": "#/$defs/ComplexExpression" },
            "rhs": { "$ref": "#/$defs/ComplexExpression" }
          },
          "required": [ "$type", "operator", "lhs", "rhs" ],
          "additionalProperties": false
        }
      ]
    },
    "BooleanExpression": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComparisonOperation" },
            "operator": {
              "type": "string",
              "enum": [ "LessThan", "GreaterThan" ]
            },
            "lhs": { "$ref": "#/$defs/NumericExpression" },
            "rhs": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "operator", "lhs", "rhs" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BooleanConditional" },
            "guard": { "$ref": "#/$defs/BooleanExpression" },
            "true_case": { "$ref": "#/$defs/BooleanExpression" },
            "false_case": { "$ref": "#/$defs/BooleanExpression" }
          },
          "required": [ "$type", "guard", "true_case", "false_case" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BooleanRangeElement" },
            "index": { "$ref": "#/$defs/NumericExpression" },
            "source": { "$ref": "#/$defs/BooleanRangeExpression" }
          },
          "required": [ "$type", "index", "source" ],
          "additionalProperties": false
        }
      ]
    },
    "LiteralNumericRange": {
          "type": "object",
          "properties": {
            "values": {
              "type": "array",
              "items": { "type": "number" }
            }
          },
          "required": [ "values" ],
          "additionalProperties": false
    },
    "NumericRangeExpression": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericRangeRegister" },
            "$ref": { "type": "string" }
          },
          "required": [ "$type", "$ref" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericRangeRegister" },
          },
          "required": [ "$type" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "LiteralNumericRange" },
            "values": {
              "type": "array",
              "items": { "type": "number" }
            }
          },
          "required": [ "$type", "values" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "SteppedRange" },
            "inclusive_start": { "$ref": "#/$defs/NumericExpression" },
            "step": { "$ref": "#/$defs/NumericExpression" },
            "exclusive_end": { "$ref": "#/$defs/NumericExpression" }
          },
          "required": [ "$type", "inclusive_start", "step", "exclusive_end" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "NumericAverageOver" },
            "source": { "$ref": "#/$defs/NumericRangeExpression" },
            "buffer_dimensions": {
              "type": "array",
              "items": { "type": "number" }
            },
            "averaging_axis": { "type": "number" },
          },
          "required": [ "$type", "source", "buffer_dimensions", "averaging_axis" ],
          "additionalProperties": false
        }
      ]
    },
    "ComplexRangeExpression": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexRangeRegister" },
            "$ref": { "type": "string" }
          },
          "required": [ "$type", "$ref" ],
          "additionalProperties": false
        },
                {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexRangeRegister" },
          },
          "required": [ "$type" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "AcquisitionComplexRangeResult" },
            "$ref": { "type": "string" }
          },
          "required": [ "$type" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "Demodulation" },
            "frame": { "$ref": "#/$defs/FrameOrReference" },
            "trace": { "$ref": "#/$defs/AcquisitionResultOrReference" }
          },
          "required": [ "$type", "frame", "trace" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "LiteralComplexRange" },
            "values": {
              "type": "array",
              "items": { "$ref": "#/$defs/Complex" }
            }
          },
          "required": [ "$type", "values" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "ComplexAverageOver" },
            "source": { "$ref": "#/$defs/ComplexRangeExpression" },
            "buffer_dimensions": {
              "type": "array",
              "items": { "type": "number" }
            },
            "averaging_axis": { "type": "number" },
          },
          "required": [ "$type", "source", "buffer_dimensions", "averaging_axis" ],
          "additionalProperties": false
        }
      ]
    },
    "BooleanRangeExpression": {
      "oneOf": [
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BooleanRangeRegister" },
            "$ref": { "type": "string" }
          },
          "required": [ "$type", "$ref" ],
          "additionalProperties": false
        },
        {
          "type": "object",
          "properties": {
            "$type": { "const": "BooleanRangeRegister" },
          },
          "required": [ "$type" ],
          "additionalProperties": false
        }
      ]
    },
    "OptionalNumericExpression": {
      "oneOf": [
        { "$ref": "#/$defs/NumericExpression" },
        { "type": "null" }
      ]
    },
    "OptionalString": {
      "oneOf": [
        { "type": "string" },
        { "type": "null" }
      ]
    },
    "OptionalBoolean": {
      "oneOf": [
        { "type": "boolean" },
        { "type": "null" }
      ]
    }
  }
}
```