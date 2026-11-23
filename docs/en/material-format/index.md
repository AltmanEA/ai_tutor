# Material Format
id: material-format
Version: 0.1.0
Status: draft
Abstraction: format
changelog-path: CHANGELOGS/material-format.changes.md

Machine-parsable format of learning material files, per the
`project-concept` hierarchy (§5–§7). This document defines the concrete
syntax deferred by `project-concept` §10 ("material format and marker
specification"), as resolved by Council session
`sess-material-format-2026-11-23`. Domain terms are defined in the
glossary.

## 1. File Model

- A learning material file is a UTF-8 Markdown file with one **topic**
  per file (a lecture comprises one or more such files).
- A **YAML frontmatter** block is mandatory as the first content of the
  file and must contain `format_version: X.Y.Z` (semantic versioning:
  patch and minor are backward compatible, major is not).
- `#` (level-1 heading) — the **topic**; exactly one per file.
- `##` (level-2 heading) — a **segment**.
- Headings deeper than level 2 are not supported.
- A new topic heading never appears after the first segment's atoms are
  open except at the file start; a segment does not cross a topic
  boundary.

## 2. Atoms

- An **atom** is a paragraph: lines separated from neighbours by at
  least one blank line. Blank lines inside fenced code blocks do not
  count.
- Atom types: **definition**, **example**, **assignment**.
- A definition is an atom without a marker; it begins with the defined
  term (§4).
- An example begins with the marker `Example (name; term):`; an
  assignment begins with `Assignment (name; term):`. The name is
  mandatory and unique across the shared name space of atom, term and
  sense names (`project-concept` §6); the term is optional.
- An example/assignment marker without a link term —
  `Example (name):` — is permitted and carries no automatic link.
- The link term in the marker is an implicit `requires` link from the
  atom to the named term (`project-concept` OQ-2 is closed this way).
- Lists and blockquotes may appear inside an atom body but may not
  begin an atom: a line starting a new block must be an atom continuation
  or a new atom per the blank-line rule.

## 3. Code

- Only fenced code blocks (``` or ~~~) are permitted as code. Indented
  code blocks are forbidden at format level: a blank line inside them
  would falsely split an atom.
- Fenced code blocks are inert: no atoms, markers, terms or links are
  recognized inside them.
- Inline code (backticks) is masked before link scanning: braces inside
  inline code are literal text, not links (§5).

## 4. Terms and Emphasis

- A defined term is marked bold: `**термин**`.
- The bold marker is recognized as a term **only inside a definition
  atom** (scoped-bold rule). Elsewhere `**...**` is ordinary Markdown
  emphasis with no format meaning.
- A definition contains exactly one bold term, at its beginning; a
  second bold run inside a definition is an error.
- Ordinary text emphasis uses `__...__` (double underscore). The
  single-underscore form `_..._` has no format meaning and is
  discouraged, being ambiguous with Markdown italics. Some renderers
  display `__...__` as bold; this accepted trade-off keeps both emphasis
  markers two characters wide.

## 5. Links

- Links are written in braces immediately after the term or marker that
  owns them, separated from it by one space: `термин {type: A, B}`.
- Grammar (flat, no nesting):
  `{` link-type `:` target ( `,` target )* `}` where
  - link-type is one of `refines`, `extends`, `requires` (lowercase);
  - target is a name/term: one or more word characters including
    Unicode letters, digits, `-`, `_`, and internal spaces.
- A definition declares links after its bold term:
  `**Функция** {requires: область определения, значение} — ...`
- A link list may contain several typed groups: `{refines: X, requires: A, B}`;
  the comma separates targets within one type; different types are
  written in one braces group, each introduced by its keyword and colon.
- `refines` and `extends` take exactly one target (`project-concept` §7).
- Parentheses `(...)` carry no format meaning: they are ordinary text.
- A link to a term not yet defined in the course is legal
  (forward reference); a link unresolvable anywhere in the curriculum
  and a link from an atom to itself are errors
  (`project-concept` §7 link closure rule).

## 6. Errors

The following are errors, not warnings (`project-concept` §10
invariant), detected at two levels:

- **Syntax (per file)**: missing or malformed frontmatter; missing or
  multiple `#` headings; a heading deeper than level 2; an
  example/assignment marker missing its name; a definition without a
  leading bold term or with a second bold run; a malformed braces link
  (unknown type, empty target, nesting, missing colon); indented code
  block; duplicate name inside the file.
- **Semantic (course-wide)**: duplicate name across the shared name
  space; unresolvable link target; self-link; `refines`/`extends` with
  more than one target.

Diagnostics name file, line and column, and tag scope
(`per-file` | `cross-file`). Per-file checks run on every commit
(pre-commit hook); course-wide checks run as a separate pass (an
incremental index is a tooling optimization, deferred).

## 7. Examples

Definition with links:

```markdown
**Функция** {requires: область определения, значение} — отображение
множества A в множество B, каждому элементу A сопоставляющий ровно
один элемент B.
```

Example with implicit requires:

```markdown
Example (fn-square; функция): f(x) = x^2. Здесь (обычная скобка —
не связь) аргумент x ∈ ℝ.
```

Assignment:

```markdown
Assignment (as-deriv-sum; производная): найдите производную
f(x) = x^2 + 3x.
```

## 8. Deferred

- Tooling: parser/validator implementation, name index, IDE feedback —
  separate session (`project-concept` §10).
- `course_id` in frontmatter: undecided (OQ-A of
  `sess-material-format-2026-11-23`).
- Canonical form of names reconciling exact-name and morphological
  matching: open (`project-concept` OQ-1).

## Cross-References

- `project-concept` §5–§7 — hierarchy, naming, links (concept level);
  heading levels there are to be aligned to this format in a separate
  Council session (operator decision 2026-11-23).
- `glossary` — domain terms.
