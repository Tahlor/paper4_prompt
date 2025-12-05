**Objective:**
For the provided image of a genealogical record, your task is to extract the information for the fields specified below. Your entire output must be a single, complete JSON object.

**Output Format & Rules:**

  * **JSON Only:** The entire response must be a single JSON object. Do not include any text or formatting outside of the JSON structure.
  * **Schema:** The JSON object must strictly follow the schema provided below.
  * **Missing Fields:** If a field listed in the schema does not appear on the document, its value must be `"N/A"`.
  * **Blank Fields:** If a field is present on the document but is left blank, its value must be an empty string `""`.

**JSON Schema Template:**

```json
{
  "SelfNamePrefix": "",
  "SelfGivenName": "",
  "SelfSurname": "",
  "SelfNameSuffix": "",
  "SelfGender": "",
  "SelfRace": "",
  "SelfMaritalStatus": "",
  "SelfDeathDay": "",
  "SelfDeathMonth": "",
  "SelfDeathYear": "",
  "SelfDeathAge": [],
  "SelfDeathCity": "",
  "SelfDeathCounty": "",
  "SelfBurialDay": "",
  "SelfBurialMonth": "",
  "SelfBurialYear": "",
  "SelfBurialCity": "",
  "SelfBurialCounty": "",
  "SelfBurialState": "",
  "SelfBurialCountry": "",
  "SelfBurialCemetery": "",
  "SelfBirthDay": "",
  "SelfBirthMonth": "",
  "SelfBirthYear": "",
  "SelfBirthCity": "",
  "SelfBirthCounty": "",
  "SelfBirthState": "",
  "SelfBirthCountry": "",
  "SpouseNamePrefix": "",
  "SpouseGivenName": "",
  "SpouseSurname": "",
  "SpouseNameSuffix": "",
  "FatherNamePrefix": "",
  "FatherGivenName": "",
  "FatherSurname": "",
  "FatherNameSuffix": "",
  "FatherBirthPlace": "",
  "MotherNamePrefix": "",
  "MotherGivenName": "",
  "MotherSurname": "",
  "MotherNameSuffix": "",
  "MotherBirthPlace": "",
  "CauseOfDeath": "",
  "FormRevision": ""
}
```

**Transcription Instructions:**

  * **Standardization:** Transcribe all values exactly as they appear on the document. Do not expand abbreviations (e.g., transcribe `Pgh.` as `Pgh.`, not `Pittsburgh`), correct spellings, or change formatting. The only exception is parsing names into their respective fields.
  * **Tokenization**: All transcribed string values (except for the special values `"N/A"` and `""`) must be segmented into single characters. Each character must be separated by a comma followed by a space. The entire resulting string must be padded with a single space at the beginning and a single space at the end.
      * **Example 1 (Name):** A value of `Mary` must be returned as `" M , a , r , y "`.
      * **Example 2 (Name):** A value of `Smith` must be returned as `" S , m , i , t , h "`.
      * **Example 3 (Name with space):** A value of `Mary Jane` must be returned as `" M , a , r , y ,   , J , a , n , e "`.
      * **Example 4 (Number):** A value of `1925` must be returned as `" 1 , 9 , 2 , 5 "`.
      * **Example 5 (Age Array):** An age of "24" years, "11" months would result in `[" 2 , 4 ", " 1 , 1 ", ""]`.
  * **Name Parsing:** If the document provides a single field for a full name, you must intelligently parse it into the appropriate `Prefix`, `GivenName`, `Surname`, and `Suffix` fields. All middle names and initials must be included as part of the `GivenName`.
  * **Death Age:** Should be formatted as `[years, months, days]`.
      * If the age unit is not specified, assume it is years. Use `""` to represent any missing parts of the age. For example, an age of "X months, Y days" should be `["", "X", "Y"]`.
      * Remember to apply the **Tokenization** rule to these string values (e.g., `[" 2 , 4 ", " 1 , 1 ", ""]`).
  * **Ditto Marks (''):** When a field contains a ditto mark (e.g., ' " ” `do` `ibid`), transcribe the value from the field directly above it. The transcribed value must be enclosed in a set of single apostrophes.
      * For example, if `FatherBirthPlace` is `"Lorem Ipsum"` and the `MotherBirthPlace` field below it contains a ditto mark, the value for `MotherBirthPlace` must be `"' L , o , r , e , m ,   , I , p , s , u , m '"`. (Note the tokenization *inside* the single quotes).
  * **FormRevision:** Transcribe the certificate number, form type, and revision number from the document, if present.
