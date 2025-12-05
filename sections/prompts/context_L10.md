### Task Overview:

For the attached image of a tabular list of names (typically two name columns per row), extract each row into a JSON array entry containing the exact fields listed below.

### Output Requirements (exact):

- Output nothing but a single, standalone JSON entity: an array of objects. No surrounding text, no Markdown, no explanation.
- Each array element corresponds to one row in the table (left-to-right order).

### Fields (each object must contain exactly these keys):
SelfGivenName, SelfSurname, SelfNamePrefix, SelfNameSuffix, OtherGivenName, OtherSurname, OtherNamePrefix, OtherNameSuffix

Example output shape:
[
  {
    "SelfGivenName": "",
    "SelfSurname": "",
    "SelfNamePrefix": "",
    "SelfNameSuffix": "",
    "OtherGivenName": "",
    "OtherSurname": "",
    "OtherNamePrefix": "",
    "OtherNameSuffix": ""
  },
]

### Interpretation Bias (Scale 1-10): 10 - Heavy Context

This setting controls how you balance literal visual transcription versus contextual inference.

- **Level 1 (Literal Visuals):** This is a pure character-level transcription. You must assess each character in isolation and transcribe *only* the literal marks you see. Do not use any contextual knowledge (e.g., common names, geography, field types) to correct, guess, or infer the value. If the result is misspelled or nonsensical, it must be transcribed as-is.
- **Level 5 (Balanced):** Use a mix of literal character transcription and contextual inference. Read the characters first, but use your knowledge of common names, dates, and field types to resolve ambiguities (e.g., "J-a-n-u-?-r-y" in a 'Month' field is inferred to be "January").
- **Level 10 (Heavy Context):** Rely primarily on your contextual and language model. If characters are highly ambiguous or illegible, your "best guess" based on what is *plausible* for that field (e.g., a known city, a common surname that matches other family members) should take precedence over a messy literal transcription.

**Again, you are Level 10 - Heavy Context.**

### Transcription & Parsing Rules (mandatory):

1.  **Row Inclusion:** Only create a JSON object for a row if at least one of the two name columns contains a non-trivial name. **Skip any row where *both* name columns are either blank or contain only a ditto/repeat mark.**

2.  **Pure JSON only:** Produce only the JSON array described above. If a field is not present in the image, set its value to an empty string `""`.

3.  **Column mapping:** The first name column (left-most name column) maps to the "Self*" fields. The second name column (to its right) maps to the "Other*" fields.

4.  **Ditto / repeated marks:** If a cell contains a repeated-entry indicator (for example: a single or double quote `, two quotes ""`, an abbreviation like `do`, or other marks meaning "same as above"), do not copy the previous text. Instead return exactly the string `"@"` for the GivenName field and an empty string `""` for the Surname field for that person.
    -   If Self column shows a ditto: set `"SelfGivenName": "@"` and `"SelfSurname": ""`.
    -   If Other column shows a ditto: set `"OtherGivenName": "@"` and `"OtherSurname": ""`.

5.  **Splitting names into Given / Surname / Prefix / Suffix:**
    a.  **Preserve Verbatim:** Preserve every character and punctuation exactly as shown (do not normalize abbreviations or expand periods). Copy punctuation and capitalization verbatim.
    b.  **Prefix:** Titles such as "Mr", "Mrs", "Miss", "Ms", "Dr", "Rev." etc. that appear *before* the name — place them into the respective `*NamePrefix` field, exactly as written.
    c.  **Suffix:** Items like "Jr", "Sr", "III", "PhD", etc. that appear *after* the name — place into the respective `*NameSuffix` field, exactly as written.
    d.  **Comma Rule (Highest Priority):** If the cell (after extracting prefix/suffix) contains a comma (e.g., "LASTNAME, First M."), this rule *always* applies. Treat it as "Surname, GivenName". Everything left of the *first* comma -> `Surname` (trim spaces), everything right -> `GivenName` (trim spaces).
    e.  **Determining Page Convention (No Comma):** For all cells *without* a comma (and after prefix/suffix extraction), you must first determine the dominant name convention for the *entire page* (either "Surname-First" or "Given-First") and apply it consistently.
        i.   **Analyze Clues:** Examine *all* multi-word names on the page to find the best-fit convention. Weigh the following clues:
            -   **Structural Clues:** Prefixes (Mr, Mrs, Dr) *before* a name or Suffixes (Jr, Sr) *after* a name *strongly* indicate a **Given-First** ("Given... Surname") convention (e.g., "Mr John Smith", "John Smith Jr").
            -   **Pattern Clues:** Look for patterns in word commonality. If the *first* word of most names is a common surname (e.g., Smith, Jones, Davies) and the *second/subsequent* words are common given names (e.g., John, William, Mary, Elizabeth) or initials (e.g., J., W. M.), assume a **Surname-First** ("Surname Given...") convention.
            -   **Ambiguity:** If the clues are contradictory or very weak, and a convention cannot be confidently determined, **default to the Surname-First convention** ("Surname Given...") as this is common in ledgers.
        ii.  **Apply Convention:** Once the page-level convention is determined, apply it to all multi-word, no-comma names:
            -   If **Given-First** convention: The *last word* is the `Surname`, and all preceding words are the `GivenName`.
            -   If **Surname-First** convention: The *first word* is the `Surname`, and all subsequent words are the `GivenName`.
    f.  **Corporate/Group Names:** This rule *overrides* 5.e. If a cell contains text that is clearly not a person's name (e.g., "Company & Co", "Org & Sons Ltd", "School Charity", "Reps of...", "...& others", "Self"), place the *entire string* into the `Surname` field and set `GivenName` to `""`.
    g.  **Single-Word Names:** This rule *overrides* 5.e. If a cell contains *only one word* (e.g., "Harker", "Jeff", "Tulling") after prefix/suffix removal, and is not a corporate/group name (like "Self"), place that single word into the `Surname` field and leave `GivenName` as `""`.

6.  **Empty / missing cells:** If the cell is present but blank, set the corresponding fields to an empty string `""`.

7.  **Line-through rows:** Transcribe the text as it appears (including any line-through). Do not remove or correct strike-throughs. Rows that have a line crossed through should still be transcribed as normal, following all other rules.

8.  **Preserve repeated punctuation or spacing** as shown. Do not correct OCR-style errors.

9.  **Output order:** Preserve the order of rows top-to-bottom. For each row create one object exactly with the stated keys in the JSON output.

### Final note:

Be conservative and literal. Do not add or remove words. Follow the splitting-convention, ditto-mark, and row-skipping rules strictly. The output must be valid JSON (a single array) and nothing else.
