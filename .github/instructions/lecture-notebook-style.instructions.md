---
description: "Use when creating beginner-to-expert lesson notebooks, Jupyter teaching materials, lecture guides, or course folders. Covers notebook JSON structure, section order, naming conventions, tone, and teaching depth."
applyTo: "**/*.ipynb, **/Lecture_guide.md"
name: "Lecture Notebook Style"
---
# Lecture Notebook Style

Use these rules when creating lesson notebooks and `Lecture_guide.md` files for beginner-to-expert course materials.

## Naming Convention

- Store each course under `courses/<topic_slug>/`.
- Store each lesson in its own folder using `NN_lesson_slug` format.
- The notebook filename must match the lesson folder name.
- Each lesson folder should also include a practice notebook named `NN_lesson_slug_practice.ipynb`.
- Add one final project folder at the end using `NN_final_project_<topic_slug>` format.
- The final project folder should contain `NN_final_project_<topic_slug>.ipynb` and `NN_final_project_<topic_slug>_solution.ipynb`.
- Valid example:
  - `courses/docker_for_data_scientist/01_intro_to_docker/01_intro_to_docker.ipynb`
  - `courses/docker_for_data_scientist/01_intro_to_docker/01_intro_to_docker_practice.ipynb`
  - `courses/docker_for_data_scientist/01_intro_to_docker/Lecture_guide.md`
- Final project example:
  - `courses/docker_for_data_scientist/08_final_project_docker_workflow/08_final_project_docker_workflow.ipynb`
  - `courses/docker_for_data_scientist/08_final_project_docker_workflow/08_final_project_docker_workflow_solution.ipynb`
  - `courses/docker_for_data_scientist/08_final_project_docker_workflow/Lecture_guide.md`
- Use 2-digit numbering, lowercase letters, and snake_case.
- Avoid generic names such as `lesson1.ipynb`, `intro.ipynb`, or `notebook.ipynb`.

## Notebook Format

- Notebook content must be valid JSON.
- The root object must contain a `cells` array.
- Each cell must be a valid JSON object.
- Each cell must include `metadata.language` matching the content type, such as `markdown` or `python`.
- Existing cells must preserve `metadata.id`.
- New cells do not need `metadata.id`.
- Do not write notebook content as loose Markdown outside notebook JSON structure.

## Teaching Style

- Write all notebook teaching content in English.
- Use a calm, professional, beginner-friendly teaching voice.
- Explain ideas from intuition to mechanism to practical usage.
- Prefer simple examples first, then increase complexity gradually.
- Avoid icons, emoji, decorative separators, and exaggerated marketing language.
- Make each lesson feel complete, not like lecture notes copied from slides.

## Required Notebook Sections

Each lesson notebook should follow this flow:

1. Title and learning objectives
2. Why this topic matters
3. Intuition-first explanation
4. Core concepts step by step
5. Practical examples or walkthroughs
6. Real-world usage for data scientists or adjacent roles
7. Common mistakes and misconceptions
8. Mini practice, reflection, or thought exercise
9. Recap
10. Bridge to the next lesson

## Practice Notebook Rules

- Each lesson must have a separate practice notebook.
- Each practice notebook should contain 7 to 15 questions or exercises depending on lesson scope.
- Include a balanced mix of recall, explanation, code reading, debugging, and small application tasks when appropriate.
- Include an answer key with concise reasoning for every question.
- Keep the practice notebook in English and beginner-friendly.

Suggested flow:

1. Title and practice goals
2. Instructions for the learner
3. Questions 1..n
4. Answer key with concise explanations

## Code Example Rules

- Prefer Python examples when the topic allows it.
- If the topic is not purely Python-based, connect examples back to a realistic data scientist workflow.
- Keep examples short enough for beginners to follow line by line.
- Add short explanatory Markdown before or after code when needed.
- Favor examples that can run locally with minimal setup.

## Final Project Rules

- Add one final project folder after the main lessons.
- The project brief notebook should define the scenario, task, constraints, deliverables, evaluation criteria, and suggested approach.
- The solution notebook should provide one strong reference solution with explanations and trade-offs.
- The project should synthesize multiple lessons rather than repeating a single isolated concept.
- Keep both project notebooks in English and at a level appropriate for someone who completed the full course.

## Lecture Guide Rules

Each `Lecture_guide.md` should include:

- Lesson promise
- Teaching storyline
- Key points to emphasize
- Common confusion points
- Useful analogies or mental models
- Suggested demo flow
- Practical job relevance talking points
- Strong closing and transition to the next lesson

The writing should sound natural when spoken aloud during teaching or video recording.