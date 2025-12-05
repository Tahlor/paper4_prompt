**Objective:**
For the provided image of a genealogical record, your task is to perform a multi-step transcription. You will first generate an initial draft, then critically review that draft in prose, and finally produce a single, complete JSON object with your refined findings.

**Required Output Structure:**

Your response MUST follow this exact format structure. Your response should start directly with the section header (no code block wrapper):

### Initial Transcription Draft
```json
{
  "FIELD_NAME": {
    "value": "TEXT_VALUE",
    "legibility": LEGIBILITY_SCORE,
    "validity": VALIDITY_SCORE
  },
  "ANOTHER_FIELD": {
    "value": "TEXT_VALUE",
    "legibility": LEGIBILITY_SCORE,
    "validity": VALIDITY_SCORE
  }
}
```

### Critical Review

[Your prose analysis here, thinking through the transcription...]

### Final JSON Output
```json
{
  "FIELD_NAME": {
    "value": "TEXT_VALUE",
    "legibility": LEGIBILITY_SCORE,
    "validity": VALIDITY_SCORE
  },
  "ANOTHER_FIELD": {
    "value": "TEXT_VALUE",
    "legibility": LEGIBILITY_SCORE,
    "validity": VALIDITY_SCORE
  }
}
```

**CRITICAL FORMATTING RULES:**
- Each JSON code block MUST be closed with ``` on a new line after the JSON object ends
- There MUST be a blank line between the closing ``` and the next section header
- The Final JSON Output must be the only content in that section (no text before or after the JSON)

**Section Requirements:**

1. **Initial Transcription Draft:**
   - A JSON code block containing your first-pass transcription for all required fields
   - This draft should use the final JSON schema, but represents your "quick guess" before deep analysis

2. **Critical Review:**
   - A prose (text) analysis of your initial draft
   - You must critically re-examine your own draft against the image
   - **Do not** just state your reasoning. Instead, *show your work* by "thinking out loud"
   - Explicitly challenge your own assumptions
   - Discuss ambiguities, ink splotches, and contextual clues
   - This section is for "shaking" your own confidence to arrive at a more accurate, well-considered final answer

3. **Final JSON Output:**
   - The entire, final, and complete JSON object
   - This JSON must incorporate all the corrections and insights from your "Critical Review" step

**Output Format & Rules:**

  * **Schema:** The JSON object must strictly follow the schema provided below.
  * **Missing Fields:** If a field listed in the schema does not appear on the document, its value must be "N/A". `legibility` and `validity` should reflect the certainty that the field is missing.
  * **Blank Fields:** If a field is present on the document but is left blank, its value must be an empty string "". `legibility` and `validity` should reflect the certainty that the field is blank.

**Required Fields:**
SelfNamePrefix, SelfGivenName, SelfSurname, SelfNameSuffix, FatherNamePrefix, FatherGivenName, FatherSurname, FatherNameSuffix, FatherBirthPlace, MotherNamePrefix, MotherGivenName, MotherSurname, MotherNameSuffix, MotherBirthPlace

**Field Format:**
Each field must be a dictionary with "value" (string), "legibility" (integer 1-10), and "validity" (integer 1-10). For example:

```json
{
  "SelfGivenName": {
    "value": str, 
    "legibility": int, 
    "validity": int 
  }
}
```

**Transcription Instructions**

  * **Standardization:** Transcribe all values exactly as they appear on the document. Do not expand abbreviations, correct spellings, or change formatting. The only exception is parsing names into their respective fields.
  * **Name Parsing:** If the document provides a single field for a full name, you must intelligently parse it into the appropriate Prefix, GivenName, Surname, and Suffix fields. All middle names and initials must be included as part of the GivenName.
  * **Ditto Marks (''):** When a field contains a ditto mark (e.g., ' " ” do ibid), transcribe the value from the field directly above it. The transcribed value must be enclosed in a set of single apostrophes. For example, if FatherBirthPlace is "Lorem Ipsum" and the MotherBirthPlace field below it contains a ditto mark, the value for MotherBirthPlace must be "'Lorem Ipsum'".

**Scoring Instructions:**

**1. Legibility Score (1-10)**
Assess strictly how clearly visible and readable the text is physically on the page. Ignore the meaning of the text; focus only on the visual clarity.

  * **10 (Perfect):** High-contrast, sharp handwriting or print. No ambiguity in character shape.
  * **7-9 (Good):** Clear text, but perhaps slight fading, light ink, or minor handwriting quirks.
  * **4-6 (Moderate):** Smudged, faded, or messy handwriting. Some characters are hard to distinguish (e.g., 'e' vs 'c').
  * **1-3 (Poor):** Heavily damaged, obscured, crossed out, or nearly illegible scrawl. Guesswork required.

**2. Validity Score (1-10)**
Assess how well the extracted value fits the field definition and linguistic norms.

  * **10 (Perfect):** A common name, standard dictionary word, or a value you expect to find in that field.
  * **8-9 (High):** A standard abbreviation or a known but less common name.
  * **4-7 (Medium):** Uncommon spellings, phonetic spellings, or names that are plausible but rare.
  * **1-3 (Low):** Nonsense strings, random characters, or data types that do not match the field (e.g., numbers in a name field).
