# Glossary
id: glossary
Version: 0.1.0
Status: draft
Abstraction: conceptual
changelog-path: CHANGELOGS/glossary.changes.md

Canonical domain terms of the ai_tutor project. The English form is
authoritative; the Russian form guides the translated mirror
(`docs/ru/glossary/index-RU.md`).

## 1. Knowledge

- **sense** (смысл) — Information containing elements and connections
  between them that fits into the working memory of a human brain.
- **concept** (понятие) — A unit of knowledge uniting several senses of
  one entity, denoted in language by a term.
- **term** (термин) — A word by which a concept is denoted in language.
- **name** (имя) — A unique identifier of an atom, a term or a sense
  within a course.

## 2. Learning material structure

- **atom** (атом) — The minimal unit of learning material: a definition,
  an example or an assignment.
- **definition** (определение) — An atom that introduces a term and the
  main sense of a concept.
- **example** (пример) — An atom that illustrates the use of a term.
- **assignment** (задание) — An atom that asks the Student to perform
  practical work to consolidate or check a sense.
- **topic** (тема) — A section of a lecture containing several segments.
- **segment** (сегмент) — A part of a topic: a group of atoms, one
  portion of new material at an explanation step.
- **lecture** (лекция) — A file of learning material; the top level of
  the material hierarchy, consisting of topics.

Material hierarchy: lecture → topic → segment → atom.

## 3. Links

- **link** (связь) — A directed reference from one atom to another,
  expressing a sense dependency between them.
- **refines** (уточняет; связь «уточнения») — A directed link type by
  which a new concept (denoted by a new term) is introduced by adding a
  refining sense to the source concept.
- **extends** (расширяет; связь «расширения») — A directed link type by
  which a new sense (with its own unique name in the name space) is
  added to an existing concept without introducing a new term.
- **requires** (опирается; связь «опора») — A directed link type
  indicating that understanding a definition requires prior
  understanding of the target term (concept).

## 4. Learning process

- **progress** (прогресс) — The recorded state of mastery (level of
  command) of each sense by a given Student, tracked over time with
  account of memory dynamics.
- **lesson** (занятие) — A continuous stretch of the learning process
  during which the Student studies a given volume of material; consists
  of a sequence of steps.
- **step** (шаг) — The minimal unit of the learning process, within
  which progress events are recorded once.

## 5. Roles

- **Professor** (Профессор; human) — Sets the teaching concept and
  methodology and the core content of the course; the authority over
  the curriculum; may intervene at any point.
- **Docent** (Доцент; AI agent) — Acts as the teacher during study:
  explains, forms assignments and checks, controls mastery and adjusts
  the program under the Professor's oversight.
- **Assistant** (Ассистент; AI agent) — Helps the Professor prepare
  materials: drafts of definitions, examples and assignments, an
  approximate study order.
- **Technician** (Лаборант; software) — Performs well-algorithmizable
  tasks in aid of the Assistant and the Docent.

## 6. Projects and participants

- **Core Project** (Проект ядра; Ядро / Core) — The repository and the
  set of artifacts that define the teaching methodology and the
  learning tools.
- **Learning Project** (Учебный проект) — The target project (the
  course working directory) into which Core components are deployed
  and in which learning materials and Student progress data are stored.
- **Student** (Студент) — A human who studies the learning materials of
  a course; the subject for whom progress is recorded.
