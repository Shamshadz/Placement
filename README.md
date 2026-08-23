# Job-Switch Interview Notes

A revision-focused knowledge base for a backend engineer with 1.2 years of experience and personal AI/CV projects.

## How to use this repository

- One concept gets one Markdown note. Prefer focused notes over topic dumps.
- Each area has an `INDEX.md` for quick revision and note links.
- Use [the note template](docs/note-template.md) for new concepts. Capture the interview answer, trade-offs, and a real example—not just definitions.
- Use `status: draft | reviewed | confident` and update `last-reviewed` when revising.

When you tell me **store** followed by the topics/concepts you covered, I will add or update the appropriate notes and indexes, then update this map when needed.

## Repository map

| Area | What belongs here | Starting point |
| --- | --- | --- |
| [Backend](Backend/) | APIs, auth, caching, messaging, testing | [index](Backend/INDEX.md) |
| [System Design](System-Design/) | HLD, LLD and distributed-system design | [index](System-Design/INDEX.md) |
| [AI / ML](AI_ML/) | ML, computer vision and personal AI projects | [index](AI_ML/INDEX.md) |
| [AI System Design](AI_ML/AI-System-Design/) | RAG, inference, evaluation and AI reliability | [index](AI_ML/AI-System-Design/INDEX.md) |
| [Data Engineering](Data-Engineering/) | pipelines, warehousing, batch/streaming and quality | [index](Data-Engineering/INDEX.md) |
| [DevOps](DevOps/) | Docker, AWS, Kafka, CI/CD and observability | [index](DevOps/INDEX.md) |
| [Databases](Database/) | SQL, schema design, indexing and transactions | [index](Database/INDEX.md) |
| [Core CS](Core/) | OS, DBMS, networks and OOP | [index](Core/INDEX.md) |
| [DSA](Data%20structure%20%26%20Algorithm/) | patterns, algorithms and solved problems | [index](Data%20structure%20%26%20Algorithm/INDEX.md) |
| [Behavioral & CV](Behavioural/) | STAR stories, CV/project talking points | [index](Behavioural/INDEX.md) |
| [Frontend](Frontend/) | JavaScript/React concepts relevant to role discussions | [index](Frontend/INDEX.md) |
| [Documentation](docs/) | templates and conventions | [structure guide](docs/STRUCTURE.md) |

## Note structure

```text
<Area>/
  INDEX.md                 # topic-level revision checklist and note links
  notes/
    <concept>.md           # one interview-ready concept
  projects/
    <project-name>/
      README.md            # problem, architecture, trade-offs, lessons
```

For design questions use `designs/<system>.md`; for DSA use `problems/<pattern>/<problem>.md`. Existing material can remain in its current subfolders.

## Legacy learning resources

- [DSA roadmap](https://whimsical.com/dsa-7hkpLXjD89iEdWxegQhKfD)
- [Java roadmap](https://whimsical.com/java-bWvCAvJP6rz7nrS6JAoHF), [Python roadmap](https://whimsical.com/python-3YSaQwixgYWR71FdsEZoJE), [SQL roadmap](https://whimsical.com/sql-VKNsH23oHXirch48uqoeHd)
- [DBMS roadmap](https://whimsical.com/dbms-roadmap-4wqSBUYzVA3FQJzudFtCgH), [OS cheatsheet](https://whimsical.com/operating-system-cheatsheet-6RJ6mEPvLMEBdzRgoVL1Nj), [networking cheatsheet](https://whimsical.com/networking-cheatsheet-by-love-babbar-BnnR7yyfx5P6nY3zCsUZ15)
- [Docker roadmap](https://whimsical.com/docker-VLYsnKa7EywcQmzcR89gVo), [LLD roadmap](https://whimsical.com/low-level-system-design-Cj9rbaqXbrRE6nudg23pCb), [ML roadmap](https://whimsical.com/machine-learning-QqM6jaXGdhQocjgEqtyj1r)
