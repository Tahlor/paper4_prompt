# Death Record Transcription - Confidence Score

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
  "SelfNamePrefix": {"value": "", "confidence": 0.00},
  "SelfGivenName": {"value": "", "confidence": 0.00},
  "SelfMaidenName": {"value": "", "confidence": 0.00},
  "SelfSurname": {"value": "", "confidence": 0.00},
  "SelfNameSuffix": {"value": "", "confidence": 0.00},
  "SelfGender": {"value": "", "confidence": 0.00},
  "SelfRace": {"value": "", "confidence": 0.00},
  "SelfMaritalStatus": {"value": "", "confidence": 0.00},
  "SelfDeathDay": {"value": "", "confidence": 0.00},
  "SelfDeathMonth": {"value": "", "confidence": 0.00},
  "SelfDeathYear": {"value": "", "confidence": 0.00},
  "SelfDeathAge": {"value": [], "confidence": 0.00},
  "SelfDeathCity": {"value": "", "confidence": 0.00},
  "SelfDeathCounty": {"value": "", "confidence": 0.00},
  "SelfBurialDay": {"value": "", "confidence": 0.00},
  "SelfBurialMonth": {"value": "", "confidence": 0.00},
  "SelfBurialYear": {"value": "", "confidence": 0.00},
  "SelfBurialCity": {"value": "", "confidence": 0.00},
  "SelfBurialCounty": {"value": "", "confidence": 0.00},
  "SelfBurialState": {"value": "", "confidence": 0.00},
  "SelfBurialCountry": {"value": "", "confidence": 0.00},
  "SelfBurialCemetery": {"value": "", "confidence": 0.00},
  "SelfBirthDay": {"value": "", "confidence": 0.00},
  "SelfBirthMonth": {"value": "", "confidence": 0.00},
  "SelfBirthYear": {"value": "", "confidence": 0.00},
  "SelfBirthCity": {"value": "", "confidence": 0.00},
  "SelfBirthCounty": {"value": "", "confidence": 0.00},
  "SelfBirthState": {"value": "", "confidence": 0.00},
  "SelfBirthCountry": {"value": "", "confidence": 0.00},
  "SpouseNamePrefix": {"value": "", "confidence": 0.00},
  "SpouseGivenName": {"value": "", "confidence": 0.00},
  "SpouseSurname": {"value": "", "confidence": 0.00},
  "SpouseMaidenName": {"value": "", "confidence": 0.00},
  "SpouseNameSuffix": {"value": "", "confidence": 0.00},
  "FatherNamePrefix": {"value": "", "confidence": 0.00},
  "FatherGivenName": {"value": "", "confidence": 0.00},
  "FatherSurname": {"value": "", "confidence": 0.00},
  "FatherNameSuffix": {"value": "", "confidence": 0.00},
  "FatherBirthPlace": {"value": "", "confidence": 0.00},
  "MotherNamePrefix": {"value": "", "confidence": 0.00},
  "MotherGivenName": {"value": "", "confidence": 0.00},
  "MotherMaidenName": {"value": "", "confidence": 0.00},
  "MotherSurname": {"value": "", "confidence": 0.00},
  "MotherNameSuffix": {"value": "", "confidence": 0.00},
  "MotherBirthPlace": {"value": "", "confidence": 0.00},
  "CauseOfDeath": {"value": "", "confidence": 0.00},
  "FormRevision": {"value": "", "confidence": 0.00}
}
```

## Confidence Score Instructions
For each field, assign a confidence score between 0.0 and 1.0 for the correctness of your transcription. Use exactly 2 decimal places (e.g., 0.95, 0.73, 0.12).

**Confidence Guidelines**:
- **0.9-1.0**: Extremely clear, no ambiguity
- **0.7-0.89**: Clear with minor uncertainty
- **0.5-0.69**: Moderate confidence, some ambiguity
- **0.3-0.49**: Low confidence, significant ambiguity
- **0.0-0.29**: Very uncertain, mostly guessing

## Transcription Instructions
- **Standardization**: Transcribe all values exactly as they appear on the document. Do not expand abbreviations, correct spellings, or change formatting. The only exception is parsing names into their respective fields.
- **Name Parsing**: If the document provides a single field for a full name, you must intelligently parse it into the appropriate Prefix, GivenName, Surname, and Suffix fields. All middle names and initials must be included as part of the GivenName.
- **Maiden Names**: If a woman's name is explicitly identified as her maiden name, populate the appropriate field (MotherMaidenName, SpouseMaidenName, SelfMaidenName).
- **Death Age**: Should be formatted as [years, months, days]. If the age unit is not specified, assume it is years. Use "" to represent any missing parts of the age.
- **Ditto Marks**: When a field contains a ditto mark (e.g., ", //, or do), transcribe the value from the field directly above it. The transcribed value must be enclosed in a set of single apostrophes.
- **FormRevision**: Transcribe the certificate number, form type, and revision number from the document, if present.
