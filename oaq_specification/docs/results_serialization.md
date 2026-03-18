# JSON Output Format

On job success, the user can expect to receive the requested results similar to the following example:
``` json 
{
    "version": "0.1.0",
    "compatible_version": "0.1.0",
    "boolean_range_results": [
      	{
          	"name": "range_1",
          	"value": [false, true, true, false]
    	}
    ],
    "numeric_range_results": [
      	{
        	"name": "range_1",
          	"value": [0, 0.1, 0.2]
        },
      	{
        	"name": "range_2",
          	"value": [3, 2, 1]
    	}
    ],
    "complex_range_results": [
        {
            "name": "measurements",
            "value": [[4, 0], [5, 0.2], [9, 103]]
        }
    ]
}
```
Note:
* Each top-level key ("numeric_outputs" etc.) is optional.
* The keys of the items in each top-level map are the names of the registers defined in the job, so in this example a NumericRangeRegister was defined with the name "range_1". 


## JSON Schema
``` json
{
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "type": "object",
    "properties": {
        "version": {"type": "string"},
        "compatible_version": {"type": "string"},
        "boolean_range_results": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "name": {"type": "string"},
                    "value": {
                        "type": "array",
                        "items": {"type": "boolean"}
                    }
                },
                "required": ["value"],
		        "additionalProperties": false,
            }
        },
        "numeric_range_results": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "name": {"type": "string"},
                    "value": {
                        "type": "array",
                        "items": {"type": "number"}
                    }
                },
                "required": ["value"],
		        "additionalProperties": false,
            }
        },
        "complex_range_results": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "name": {"type": "string"},
                    "value": {
                        "type": "array",
                        "items": {
                            "type": "array",
                            "items": { "type": "number" },
                            "minItems": 2,
                            "maxItems": 2
                        }
                    }
                },
                "required": ["value"],
		        "additionalProperties": false,
            }
        }
    },
    "required": [
        "version",
        "compatible_version"
    ],
    "additionalProperties": false
}
```
