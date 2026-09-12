# abapGit DTEL Test Repository

This repository contains example DTEL (Data Element) objects in AFF JSON format for testing the abapGit DTEL serializer implementation.

## Test Files

### Simple Examples

- **zdtel_example_simple.dtel.json** - Basic character data element (CHAR, length 10) with field labels
- **zdtel_example_date.dtel.json** - Date data element (DATS, length 8)

### Predefined Type Examples

- **zdtel_example_currency.dtel.json** - Currency data element (CURR, length 15, 2 decimals)
- **zdtel_example_string.dtel.json** - Variable-length string data element (STRING, length 0)

### Reference Examples

- **zdtel_example_domain.dtel.json** - Data element based on the included ZDTEL_EXAMPLE_BASE domain
- **zdtel_example_ref_char.dtel.json** - Reference to a predefined type (CHAR, length 2)
- **zdtel_example_ref_dtel.dtel.json** - Reference to ZDTEL_EXAMPLE_SIMPLE, another data element in this repository
- **zdtel_example_ref_intf.dtel.json** - Reference to the standard IF_SERIALIZABLE_OBJECT interface

### Additional Properties

- **zdtel_example_properties.dtel.json** - Field labels, bidirectional options, default component name, change-document relevance, and disabled input history

The supporting **zdtel_example_base.doma.json** domain keeps the domain-reference example self-contained.

## Testing

To test the DTEL AFF serializer:

1. Enable the AFF experimental feature in abapGit settings.
2. Clone this repository into your SAP system using abapGit.
3. Verify that the domain and data elements are created and activated with the documented attributes.
4. Export the objects and verify that their JSON matches the expected AFF structure.

## AFF Format Structure

All DTEL files follow the SAP ABAP File Format v1 specification:

```json
{
  "formatVersion": "1",
  "header": {
    "description": "Data element description (max 60 chars)",
    "originalLanguage": "en",
    "abapLanguageVersion": "standard"
  },
  "dataTypeInformation": {
    "category": "predefinedType",
    "predefinedType": {
      "dataType": "CHAR",
      "length": 10,
      "decimals": 0
    }
  },
  "fieldLabels": {
    "short": "Short",
    "shortLength": 10,
    "medium": "Medium label",
    "mediumLength": 20,
    "long": "Long field label",
    "longLength": 40,
    "heading": "Column heading",
    "headingLength": 20
  },
  "additionalProperties": {
    "searchHelp": {
      "name": "SEARCH_HELP",
      "parameter": "PARAMETER"
    },
    "bidirectionalOptions": {
      "basicDirection": "leftToRight",
      "noFiltering": false
    },
    "parameterId": "PID",
    "defaultComponentName": "VALUE",
    "changeDocumentRelevant": false,
    "noInputHistory": false
  }
}
```

The `predefinedType` object is only used by the `predefinedType` and `referenceToPredefinedType` categories. The remaining categories name the referenced object with `typeName` instead:

```json
{
  "dataTypeInformation": {
    "category": "domain|referenceDictionaryType|referenceClasIntType",
    "typeName": "REFERENCED_TYPE"
  }
}
```

## Not Covered Yet

The examples exercise all five `category` values, but the following parts of the schema have no example file yet:

- `additionalProperties.searchHelp` - requires a search help that exists in the target system
- `additionalProperties.parameterId` - requires a Set/Get parameter ID that exists in TPARA
- `header.abapLanguageVersion` - every file relies on the `standard` default
- The boolean properties with value `false`, and `decimals` with an explicit `0`
- Partial `fieldLabels`, and field labels on the reference-category examples
- Data types other than CHAR, DATS, CURR and STRING - in particular the ones where the dictionary fixes the length, such as INT4, UTCLONG, FLTP and LANG

## Related Implementation

- DTEL AFF implementation: https://github.com/abapGit/abapGit/pull/7808
- AFF specification: https://github.com/SAP/abap-file-formats/tree/main/file-formats/dtel
