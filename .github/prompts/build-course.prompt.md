---
description: "Build a beginner-to-expert course with numbered lesson folders, professional notebook filenames, and lecture guides. Trigger: build course, create course, design lecture series, notebook course, teaching materials."
name: "build-course"
argument-hint: "Topic to teach, optional lesson count, optional audience, optional output path. Example: Docker for Data Scientist, 7 lessons for career switchers."
agent: "Beginner-to-Expert Lecture Designer (Notebook)"
---
Create a complete beginner-to-expert course from the user request.

Requirements:
- If the user does not specify a lesson count, default to 7 lessons.
- Create a full lesson series, not just a prompt list.
- Use professional lesson naming such as `01_intro_to_docker` and notebook filenames such as `01_intro_to_docker.ipynb`.
- Create one lesson folder per lesson.
- In each lesson folder, create the main notebook, a practice notebook with answers, and `Lecture_guide.md`.
- Add one final project folder at the end of the course with a project brief notebook, a solution notebook, and `Lecture_guide.md`.
- Keep all teaching content in English.
- Follow the lesson style and notebook structure rules in [lecture-notebook-style.instructions.md](../instructions/lecture-notebook-style.instructions.md).

The user request is:

{{input}}