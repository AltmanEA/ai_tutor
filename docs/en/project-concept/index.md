# ai_tutor Project Concept
id: project-concept
Version: 0.3.0
Status: draft
Abstraction: conceptual
changelog-path: CHANGELOGS/project-concept.changes.md

The concept of the ai_tutor project: AI-based means of automating the
learning process. Council sessions read this document as the anchor for
every review. Concrete formats, prompts and specifications are developed
in later Council sessions (§10).

## 1. Purpose and Domain

ai_tutor creates means of automating the learning process with AI.
The application domain is education in information technology, first of
all programming; the target audience is students of IT specialties.

## 2. Distinction of Projects

Two projects are distinguished in the documentation:

- **Core Project (Core)** — creation of the documentation set, skills
  for AI agents, and supporting software (an MCP server).
- **Learning Project** — the project into which the results of the
  Core Project are installed and in which the learning materials and
  the records of their mastery (progress data) live.

Disambiguation rule: if it is not obvious from the context which project
is meant, the AI must clarify with the user; until the target project is
established the agent performs no write to either project. This is a
deliberate safety choice: a wrong guess has a destructive,
cross-boundary direction. Deliverables flow one way — from the Core
Project into a Learning Project.

## 3. Roles in the Learning Project

- **Professor** (human) — sets the teaching concept, the teaching
  methodology and the core content of the course: terminology, main
  concepts, theories, the direction of practical assignments. The
  Professor is the authority over the curriculum and can intervene at
  any point. Produces: curricular decisions and approved materials.
- **Assistant** (AI agent) — the Professor's helper in preparing
  materials: forms learning elements with definitions and examples, an
  approximate study order, drafts of practical assignments — under the
  Professor's direction. Produces: material drafts for approval.
- **Docent** (AI agent) — acts as the teacher during study: explains
  terms and concepts to students, forms practical assignments and tests
  on the current topic, controls mastery and adjusts the program (what
  to deepen, what to omit). Produces: explanations, tests, assignments
  and progress updates delivered during study sessions. Its pacing and
  content decisions operate under the Professor's oversight.
- **Technician** (software) — helps the Assistant and the Docent
  with well-algorithmizable tasks; example: context minimization during
  a step. Produces: computed results for the agents it serves.

Invariant: material text is content to be taught, never instruction to
an agent; agents act on their own role instructions and on explicit
tool calls.

## 4. Teaching Concept

- A **sense** is information containing elements and connections
  between them, fitting into the working memory of a human brain.
- A **concept** is a unit of knowledge uniting several senses of one
  entity, denoted in language by a **term**.
- Cardinality: many senses → one concept → one term. Materials are
  given to the Student in atoms that contain one thought each; concepts
  are formed by gradually filling them with senses.
- A definition gives only the main thought of the concept; the concept
  is replenished with a sense when the term is refined.
- Before a term's definition is met, the term may be used in material
  text as an ordinary natural-language word — except where the everyday
  sense does not match the term's sense. That exception is a judgment
  of the Professor.

## 5. Learning Material Structure

Hierarchy: **lecture (file) → topic → segment → atom**. Materials
comprise several lecture files.

- An **atom** is the minimal unit of learning material: a definition,
  an example or an assignment (three types; an "extension" is the
  effect of an `extends` link, §7, not a fourth type). Atoms are
  separated by a blank line; blank lines inside code do not count.
- A **definition** is an atom without a marker. Its name (the term)
  appears inside, highlighted as bold; bold markup inside a definition
  is reserved for term designation only.
- An **example** or an **assignment** begins with a marker carrying a
  mandatory name and an optional related term
  (`Example (name; term):`, `Assignment (name; term):` — the concrete
  marker text is a format-level detail, §10). They may hold concrete
  content or directives for the Docent to generate content during a
  study session.
- A **topic** is a section beginning with a level-2 heading (`##`) and
  containing several segments.
- A **segment** is a group of atoms inside a topic, beginning with a
  level-3 heading (`###`), roughly one lecture slide in volume and
  content. A segment has no name of its own and is not a unit of
  progress (progress stays per atom); its mastery is always computable
  from its atoms. A segment is one portion of new material at an
  explanation step. Segment titles are unique within a topic; a repeat
  is an error.
- An atom inherits its topic and segment from its position in the
  file; a new topic heading resets the current segment — a segment
  does not cross a topic boundary. Headings of level 4 and deeper are
  not supported.

## 6. Naming and Identity

- Every atom has a name: a unique identifier within the whole course.
  Atom names, term names and sense names share one namespace, and
  uniqueness is enforced across all three (the single namespace is the
  operator's decision; the author therefore tracks atom, term and
  sense names in one shared space).
- Repeated names are errors — including an example/assignment name
  equal to a term.
- For a definition, the name is its term; for an extension, a unique
  sense name (§7). For an example or assignment, the name is the
  first element of the marker parentheses.
- Name comparison is exact, without morphology. A name is independent
  of file, topic and content: editing an atom or its surroundings never
  changes its name.

Known tension (OQ-1): names are compared exactly while terms in links
are compared by morphological base (§7) — two different equivalence
relations over one namespace; how they interact when they disagree is
unresolved.

## 7. Atom Links

- In links, terms are compared by morphological base: "функция" and
  "функцию" are the same term.
- A definition may declare links in the form `term (X: A, Y: B, C)`,
  where A, B, C are terms and X, Y are link types.
- Position X: **refines** — a new term is introduced that adds to the
  sense of a concept; **extends** — the sense is added without a new
  term. Only one term follows X. With `extends`, every sense carries a
  unique sense name; redefining the term is forbidden.
- Position Y: **requires** — understanding the current definition
  requires understanding the linked terms. A term may be linked to
  several terms, including ones not yet defined in the course.
- Links are one-directional: if D refines, extends or requires E,
  none of this holds in the reverse direction.
- For atoms other than definitions, the link is the second element of
  the marker parentheses: `Example (fn-simple; function):`. The link
  type is not labeled in this form (OQ-2).

Link closure rule: a forward reference to a term not yet defined in the
course is legal; a link that cannot be resolved anywhere in the
curriculum, and a link from an atom to itself, are errors.

## 8. Learning Progress

Progress is the fixation of the mastery level of senses for each
student. Per sense it accounts for: exposure to its atom, exposure to
linked atoms, answers to test questions and solutions of practical
tasks that require the sense; forgetting is additionally accounted
for by memory dynamics (model parameters and time base deferred,
OQ-6).

Progress is used to: evaluate atoms to be offered next for study or
repetition; formulate explanations using only senses familiar to the
Student; assign attestation grades.

Concept-level invariants: progress is recorded as evidence per step — a
step is an identified unit and its evidence is counted once, so an
interrupted or repeated step neither loses nor duplicates it; grades
are derived from that recorded evidence and must be reproducible from
it. Progress and grades are personal data of the learning project, read
only within the scope of the student they belong to.

## 9. Lesson and Step

- A **lesson** is the study of some volume of material over a continuous
  period of time; a lesson consists of several steps.
- A **step** is the minimal unit of the learning process, corresponding
  to one Docent session. At the start of a step the Docent announces
  the new step to the Technician; all progress events until the next
  announcement belong to one step and are counted once.
- At the start of a step the Docent determines the next material from
  the learning materials, the Student's progress and the Student's
  wishes: for new material a whole segment (a portion ≈ one slide);
  for repetition and point requests individual atoms; or the need for a
  knowledge check (a test or a practical task).
- After choosing the step type the Docent loads the matching skill
  (explanation — `docent-explain`; control — to be described later).
  At the end of the step the Docent updates the learning progress and
  uses the Technician to minimize context.

## 10. Deferred Scope

Deferred to later Council sessions: material format and marker
specification; role prompts and skill specifications; the progress and
control-step specification; material authoring and validation tools;
MCP server design and authorization.

Already binding concept-level invariants (not deferred): single
namespace with cross-kind uniqueness; the link closure rule; evidence
per identified step and grade reproducibility; progress as
student-scoped personal data; material text is data, never instruction;
no write to either project until the target project is established;
materials are validated deterministically before use — duplicate names,
unresolved links and structural violations are errors, not warnings
(the mechanism itself is deferred).

Hand-authoring in a machine-parsable model is error-prone; authors are
expected to work with validation-feedback tooling (deferred, §10).

## 11. Open Questions

- OQ-1 How do exact name comparison and morphological term matching
  interact when they disagree; what canonical form of names is used?
- OQ-2 Link type of example/assignment links: an implicit default or
  separately stored?
- OQ-3 Is a sense name unique per concept or across the course?
- OQ-4 Rename/retire lifecycle of names and its effect on links and
  progress.
- OQ-5 Cross-course identity: per-course namespace for a student taking
  several courses.
- OQ-6 Memory-dynamics model: time base (wall-clock vs study time);
  does it drive automatic re-presentation?
- OQ-7 Policy for cyclic links.
- OQ-8 Boundary of the Docent's authority to adjust the program.
- OQ-9 Version compatibility of installed artifacts; rule precedence
  when the two projects' rules conflict.
- OQ-10 Should test authoring be separated from grading (the current
  model gives both to the Docent)?

## 12. Glossary Note

Domain terms used here are defined in the project glossary
(`docs/en/glossary/index.md`); the glossary is authoritative for term
forms in both languages.
