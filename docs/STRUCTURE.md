# Notes-only repository structure

## Scope

- Store only Markdown notes and Markdown indexes.
- Keep external resources as links inside notes.
- Do not store source code, notebooks, datasets, PDFs, images, binaries, exports, or project deliverables here.

## Folder convention

Each subject folder contains:

    INDEX.md                Topic checklist and links to notes
    notes/
      concept-name.md       One interview-ready concept

Use designs/system-name.md for system-design answers, stories/story-name.md for behavioral answers, and problems/pattern/problem-name.md for DSA solution explanations. These are all Markdown notes.

## Note convention

- Use lowercase, hyphenated file names: rate-limiting.md, kalman-filter.md.
- Keep one durable concept per note.
- Use the shared note template in note-template.md.
- Keep reusable learning and revision workflows in skills.md.
- Include a one-minute answer, how it works, trade-offs, interview prompts, personal context, and references when applicable.
- Keep the parent INDEX.md updated whenever a note is created or moved.

## Repository map

The root README.md is the authoritative map of subject folders. Update it only when the folder structure changes.
