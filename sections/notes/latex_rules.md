# LaTeX Rules Checklist

- [ ] **Citations**: Always use a tilde `~` before a citation command to prevent line breaks.
    - Correct: `...as shown in recent work~\cite{author2023}.`
    - Incorrect: `...as shown in recent work \cite{author2023}.`

- [ ] **References**: Always use a tilde `~` before a reference command.
    - Correct: `Figure~\ref{fig:example}`
    - Incorrect: `Figure \ref{fig:example}`

- [ ] **Quotes**: Use proper LaTeX directional quotes.
    - Correct: ``quoted text''
    - Incorrect: "quoted text"

- [ ] **Figure/Table References**: Every figure and table included in the document MUST be referenced in the text.
    - Check for `\label{...}` in the figure/table environment.
    - Check for corresponding `\ref{...}` in the text.

- [ ] **Floats**: Use `[t]` or `[h]` or `[htbp]` for float placement, but avoid `[H]` unless absolutely necessary.

- [ ] **Math Mode**: Ensure variables are in math mode (e.g., $p < 0.05$).

- [ ] **Acronyms**: Define acronyms on first use.

- [ ] **Capitalization**: Capitalize "Figure", "Table", "Section", "Equation" when referring to specific numbered items (e.g., "in Section 3").
