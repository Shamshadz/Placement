Recommended repository organization for interview preparation

Purpose
- Use this repo as a single source of truth for notes, PDFs, code, and practice problems.
- Keep material organized by topic and type so you can quickly find items during study and mock interviews.

Top-level folders (current) and recommended usage
- AI/
  - ML/ : Put PDFs, lecture notes, notebooks, small projects, model code.
- Backend/
  - Java Backend/ : subfolders per framework (Spring/...)
  - JavaScript Backend/ : subfolders like Nestjs/, nodejs/
  - Python Backend/ : Django/
- Core/ : fundamental CS topics (CN, DBMS, OS) — store summary notes, cheatsheets, PDFs, and small examples here.
- Data structure & Algorithm/ : problems, solutions, and handwritten notes. Keep solved problems under a clear path like `problems/<topic>/` and `solutions/<topic>/`.
- Database/ : flavor-specific DB guides (MySQL/)
- DevOps/ : AWS/, Docker/, Kafka/ — store architecture diagrams, commands cheatsheets, and lab steps.
- Frontend/ : JavaScript/Reactjs/ — components, small projects, notes.
- System-Design/ : HLD/ and LLD/ — design docs, diagrams, and sample answers.

Suggested finer-grained structure inside a topic folder
- README.md : short description of what the folder contains and how to use it for study.
- notes/ : markdown or text notes, summaries, and cheat-sheets.
- pdfs/ : original PDFs and reference papers (name them with a short prefix and date if needed).
- problems/ : problem statements (text or pdf).
- solutions/ : your solutions (code and explanations). Use one subfolder per problem with files like `solution.py`, `explanation.md`.
- projects/ : small projects or demos.

Naming conventions
- Avoid spaces in file and folder names when possible; prefer hyphens or underscores (e.g., `Data-Structures` or `data_structures`).
- Files: `YYYY-MM-DD_short-description.ext` when saving notes or iterations.
- Code: `problem-name.language` (e.g., `binary-search.cpp`).

Versioning and large files
- Keep code and text in Git.
- For large PDFs, datasets, or binary files, use Git LFS or store them externally (Google Drive, OneDrive) and add a small pointer file with links.
- Do not commit compiled binaries (e.g., `a.exe`). Add them to `.gitignore` (done).

Readme and onboarding
- Add/keep a root `README.md` describing repository purpose and quick navigation.
- Add a `README.md` in each major folder explaining what to store there and a short study checklist.

Interview-specific tips
- Add a `mock-interviews/` folder with paired roleplay scripts, common behavioral questions, and performance notes.
- Add `flashcards/` (Anki export or simple markdown files) for quick review.
- Keep a `resume/` folder with `resume.pdf`, `cover-letter.md`, and a `projects.md` describing projects you might be asked about.

Next steps (optional automated improvements)
- Rename folders to remove spaces (low-risk but changes many paths). Ask before doing it.
- Add per-folder `README.md` files from a template. I can create those for top-level folders.
- Add Git LFS support if you'll store large PDFs.

If you'd like, I can:
- Create `docs/STRUCTURE.md` (done).
- Add a `.gitignore` (done).
- Add a README template and optionally add README files to top-level folders.
- Help rename folders to remove spaces and update links (I will ask before performing destructive moves).

Tell me which of the optional changes you'd like me to do next.