# Death Record Transcription - Literal Interpretation

## Objective
For the provided image of a genealogical record, your task is to extract the information for the fields specified below. Your entire output must be a single, complete JSON object.

## Output Format & Rules
- **JSON Only**: The entire response must be a single JSON object. Do not include any text or formatting outside of the JSON structure.
- **Schema**: The JSON object must strictly follow the schema provided below.
- **Missing Fields**: If a field listed in the schema does not appear on the document, its value must be "N/A".
- **Blank Fields**: If a field is present on the document but is left blank, its value must be an empty string "".

## JSON Schema Template
```json
{
  "SelfNamePrefix": {"literal_value": "", "is_best_fit": false},
  "SelfGivenName": {"literal_value": "", "is_best_fit": false},
  "SelfMaidenName": {"literal_value": "", "is_best_fit": false},
  "SelfSurname": {"literal_value": "", "is_best_fit": false},
  "SelfNameSuffix": {"literal_value": "", "is_best_fit": false},
  "SelfGender": {"literal_value": "", "is_best_fit": false},
  "SelfRace": {"literal_value": "", "is_best_fit": false},
  "SelfMaritalStatus": {"literal_value": "", "is_best_fit": false},
  "SelfDeathDay": {"literal_value": "", "is_best_fit": false},
  "SelfDeathMonth": {"literal_value": "", "is_best_fit": false},
  "SelfDeathYear": {"literal_value": "", "is_best_fit": false},
  "SelfDeathAge": {"literal_value": [], "is_best_fit": false},
  "SelfDeathCity": {"literal_value": "", "is_best_fit": false},
  "SelfDeathCounty": {"literal_value": "", "is_best_fit": false},
  "SelfBurialDay": {"literal_value": "", "is_best_fit": false},
  "SelfBurialMonth": {"literal_value": "", "is_best_fit": false},
  "SelfBurialYear": {"literal_value": "", "is_best_fit": false},
  "SelfBurialCity": {"literal_value": "", "is_best_fit": false},
  "SelfBurialCounty": {"literal_value": "", "is_best_fit": false},
  "SelfBurialState": {"literal_value": "", "is_best_fit": false},
  "SelfBurialCountry": {"literal_value": "", "is_best_fit": false},
  "SelfBurialCemetery": {"literal_value": "", "is_best_fit": false},
  "SelfBirthDay": {"literal_value": "", "is_best_fit": false},
  "SelfBirthMonth": {"literal_value": "", "is_best_fit": false},
  "SelfBirthYear": {"literal_value": "", "is_best_fit": false},
  "SelfBirthCity": {"literal_value": "", "is_best_fit": false},
  "SelfBirthCounty": {"literal_value": "", "is_best_fit": false},
  "SelfBirthState": {"literal_value": "", "is_best_fit": false},
  "SelfBirthCountry": {"literal_value": "", "is_best_fit": false},
  "SpouseNamePrefix": {"literal_value": "", "is_best_fit": false},
  "SpouseGivenName": {"literal_value": "", "is_best_fit": false},
  "SpouseSurname": {"literal_value": "", "is_best_fit": false},
  "SpouseMaidenName": {"literal_value": "", "is_best_fit": false},
  "SpouseNameSuffix": {"literal_value": "", "is_best_fit": false},
  "FatherNamePrefix": {"literal_value": "", "is_best_fit": false},
  "FatherGivenName": {"literal_value": "", "is_best_fit": false},
  "FatherSurname": {"literal_value": "", "is_best_fit": false},
  "FatherNameSuffix": {"literal_value": "", "is_best_fit": false},
  "FatherBirthPlace": {"literal_value": "", "is_best_fit": false},
  "MotherNamePrefix": {"literal_value": "", "is_best_fit": false},
  "MotherGivenName": {"literal_value": "", "is_best_fit": false},
  "MotherMaidenName": {"literal_value": "", "is_best_fit": false},
  "MotherSurname": {"literal_value": "", "is_best_fit": false},
  "MotherNameSuffix": {"literal_value": "", "is_best_fit": false},
  "MotherBirthPlace": {"literal_value": "", "is_best_fit": false},
  "CauseOfDeath": {"literal_value": "", "is_best_fit": false},
  "FormRevision": {"literal_value": "", "is_best_fit": false}
}
```

## Literal Interpretation Instructions
For each field, provide the most literal character-level interpretation of the handwriting without language correction or contextual guessing. 

Set `is_best_fit` to `true` if you believe this literal interpretation is also the best linguistic and contextual fit, and `false` if you think a different interpretation would be more appropriate given context and language knowledge.

**Focus on visual accuracy over linguistic sense**. If the handwriting appears to spell "Qxthur" but you think it should be "Arthur", transcribe "Qxthur" and set `is_best_fit` to `false`.

## Transcription Instructions
- **Standardization**: Transcribe all values exactly as they appear on the document. Do not expand abbreviations, correct spellings, or change formatting. The only exception is parsing names into their respective fields.
- **Name Parsing**: If the document provides a single field for a full name, you must intelligently parse it into the appropriate Prefix, GivenName, Surname, and Suffix fields. All middle names and initials must be included as part of the GivenName.
- **Maiden Names**: If a woman's name is explicitly identified as her maiden name, populate the appropriate field (MotherMaidenName, SpouseMaidenName, SelfMaidenName).
- **Death Age**: Should be formatted as [years, months, days]. If the age unit is not specified, assume it is years. Use "" to represent any missing parts of the age.
- **Ditto Marks**: When a field contains a ditto mark (e.g., ", //, or do), transcribe the value from the field directly above it. The transcribed value must be enclosed in a set of single apostrophes.
- **FormRevision**: Transcribe the certificate number, form type, and revision number from the document, if present.
