# Lecture Guide: Final Project - Refactoring a Data Science Script

## 1. Project Introduction

**"Welcome to the final project. You've spent the entire course learning the principles of Object-Oriented Programming. Now, it's time to use those skills to solve a real-world problem. Your mission is to transform a messy, procedural script into a clean, professional, and reusable data science pipeline. This project is your opportunity to demonstrate that you can write code that is not only correct but also well-designed."**

---

## 2. Presenting the Challenge

### Part 1: The "Before" Picture (10 mins)

1.  **Introduce the Scenario.** "Imagine you've just joined a new team. A colleague gives you a script. It works, they say. Your manager asks you to take ownership of it and make it a core asset for the team. You open the file, and this is what you see."
2.  **Walk through the messy script.** Go to the `08_final_project_refactoring_a_ds_script.ipynb` notebook.
    *   Scroll through the `run_boston_housing_analysis` function.
    *   Point out the different, disconnected parts: the complex data loading, the preprocessing, the model training, the evaluation, the file saving.
    *   **Run the script.** Show that it works and produces a result (`MSE: 24.29`) and a JSON file. "The script is not *wrong*—it gets the right answer. But it is *brittle*. It's hard to read, hard to change, and hard to reuse."
3.  **Ask guiding questions to highlight the problems.**
    *   "What if we wanted to use a different dataset? We'd have to rewrite the entire loading section."
    *   "What if we wanted to try a `RandomForestRegressor` instead? We'd have to dig into the middle of the function and be careful not to break anything."
    *   "What if we wanted to reuse just the data loading part for a different analysis? We'd have to resort to copy-pasting code, which is a recipe for bugs."
    *   "This is technical debt. Our job is to pay it down by refactoring."

### Part 2: The "After" Picture - The Requirements (10 mins)

1.  **Frame the task.** "Your task is to refactor this script into a system of collaborating objects, using the exact patterns we learned in the last lesson."
2.  **Walk through the requirements in the project brief.**
    *   **ABC:** "First, you'll define the contract. An abstract `PipelineStep` class with an `execute` method. This is the rule that all your components must follow."
    *   **Concrete Classes:** "Next, you'll build your team of specialists. A `DataLoader`, a `DataPreprocessor`, a `ModelTrainer`, and so on. Each one will be a class that follows the `PipelineStep` contract and has a single responsibility."
    *   **Pipeline Class:** "Then, you'll build the manager. A `Pipeline` class that uses composition to hold a list of these specialists."
    *   **Demonstrate Flexibility:** "The ultimate test: you must run the pipeline once with `LinearRegression` to verify your result, and then run it again with `RandomForestRegressor` just by changing the configuration. This proves your design is a success."
3.  **Clarify the deliverables.** "Your final output is a new notebook containing your fully refactored, object-oriented solution. It should be clean, readable, and meet all the requirements."

---

## 3. Guiding the Student's Thought Process

If students are stuck, guide them with these questions rather than giving the answer directly.

*   **"Where do you start?"** -> "Always start with the design. Look at the original script and draw boxes around the different responsibilities. Those boxes are your future classes."
*   **"How should data be passed between steps?"** -> "Think about what each step *needs* and what it *produces*. The `DataPreprocessor` needs the raw DataFrame from the `DataLoader`. It produces features (X) and a target (y). A Python dictionary is a great way to pass multiple, named items from one step to the next."
*   **"My `ModelTrainer` needs to give the `ModelEvaluator` the test data (X_test, y_test). How do I do that?"** -> "Your `ModelTrainer`'s `execute` method can return a dictionary containing the trained model, `X_test`, and `y_test`. The `ModelEvaluator` will then receive this dictionary as its `data` input."
*   **"How do I make the `ModelTrainer` configurable?"** -> "Remember dependency injection. Don't create the `LinearRegression` object *inside* the `ModelTrainer`. Instead, pass the model object you want to use into the `ModelTrainer`'s `__init__` method."

---

## 4. Reviewing the Solution

When walking through the solution notebook (`08_final_project_refactoring_a_ds_script_solution.ipynb`), emphasize these points.

1.  **The ABC is the foundation.** "Everything starts with this simple, powerful contract."
2.  **Each class is clean and focused.** "Look at `BostonHousingLoader`. It does one complex thing, and one thing only. It doesn't know about models or evaluation."
3.  **Data flows through dictionaries.** "Notice how we pass dictionaries between steps. This is a clean way to handle an increasingly complex set of data artifacts (from DataFrame -> to {X, y} -> to {model, X_test, y_test})."
4.  **The `Pipeline` class is 'dumb' (and that's a good thing!).** "The `Pipeline` class is beautifully simple. It has no specific knowledge of housing data or linear regression. It only knows how to loop through a list of steps. This is what makes it so reusable."
5.  **The final execution block is declarative.** "Look at the assembly section. The code reads like a configuration file. We are *declaring* what we want our pipeline to be. This is the ultimate goal: separating the 'what' (the configuration) from the 'how' (the execution logic inside the classes)."
6.  **Compare the results.** Show that the MSE for the `LinearRegression` in the new pipeline (24.29) exactly matches the original script. Then, point to the `RandomForestRegressor` result as the proof of a successful, flexible design.

---

## 5. Strong Closing for the Course

"Completing this project proves you are more than just a data analyst who can write scripts. You are now a data scientist who can build systems. You understand how to manage complexity, how to write code that is built to last, and how to apply software engineering principles to data science problems.

This skill is what separates good data scientists from great ones. It's what allows you to build reliable, production-ready models and to work effectively in a team. Congratulations on this significant achievement. You are now equipped with the tools and mindset to build professional-grade data science applications."
