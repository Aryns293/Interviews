# Round 5 — LLD & Light System Design
**Interview:** EdTech / Learning Platform Company
**Duration:** 60 minutes

---

## LLD — Course/Assignment/Submission System
- Course, Module, Assignment (quiz, code, video types), Submission, Grade.
- **Pattern:** Strategy (different grading strategies per assignment type) → Observer (a progress-tracker updating when any submission is graded, without grading logic knowing about the tracker).
- **Thread-safety:** 500 students submit in the last 60 seconds before a deadline — guarantee every submission is captured and correctly timestamped, none silently dropped.

---

## Light HLD — Live Virtual Classroom Feature
"Design a Live Virtual Classroom feature"
- One mentor's video/audio + shared editor to a few hundred students.
- Mostly one-way with occasional interaction.
- Push on fan-out cost, and why it's architecturally different from CodeSync AI's small-group bidirectional model.
