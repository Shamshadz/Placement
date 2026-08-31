# Study and revision skills

Reusable skills for maintaining concise, interview-ready notes in this repository. Add each new skill as a numbered section and link to it from the relevant subject index.

## 1. Structure a revision note

1. Start with a one-minute answer: what it is, why it is used, and where it fits in a pipeline.
2. Explain the flow, mathematics, or decision process with one concrete example.
3. Record failure modes and trade-offs; interviewers use these to test depth.
4. Add project-specific evidence: model, data, metric, latency/cost, outcome, and what you would improve.
5. End with likely interview questions and a checkbox for whether you can explain it without notes.

Use the shared [note template](note-template.md). Keep each note narrow, give it a lowercase hyphenated name, and add it to its parent INDEX.md.

## 2. Write detailed notes with diagrams

Use this skill when creating or substantially expanding a technical note.

1. Start with a concise one-minute interview answer, then build from intuition to implementation-level detail.
2. Explain every major component, input/output, data flow, and important mathematical relation. Define unfamiliar terms before relying on them.
3. Include at least one diagram when it materially clarifies structure, sequence, or data flow. Prefer Mermaid diagrams because GitHub renders them from Markdown.
4. Add a concrete worked example. For ML notes, include tensor shapes, a small calculation, or a training/inference trace where relevant.
5. Record alternatives, trade-offs, constraints, failure modes, debugging signals, and production implications.
6. Finish with interview questions, a project-specific explanation prompt, references, and a revision checklist.

Use this depth structure when relevant:

    One-minute answer
    Intuition
    Architecture or flow diagram
    Detailed components
    Mathematics, algorithm, or lifecycle
    Worked example
    Trade-offs and failure modes
    Production considerations
    Interview questions
    References and revision checklist

Example Mermaid diagram:

    flowchart LR
      A[Input] --> B[Processing]
      B --> C[Output]

Do not add a diagram merely as decoration. The diagram must explain a relationship that would be harder to understand from prose alone.

## Adding a future skill

Use this structure:

    ## <number>. <skill name>

    What capability this practice builds.

    1. First action
    2. Next action
    3. Final action

    Related notes: [concept](../area/notes/concept.md)

Skills describe how to learn, explain, compare, or revise a topic. Concepts, definitions, and technical details belong in the subject notes.
