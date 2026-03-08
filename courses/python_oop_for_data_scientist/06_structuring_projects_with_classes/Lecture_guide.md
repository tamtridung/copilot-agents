# Lecture Guide: Structuring Projects with Classes

## 1. Lesson Promise

**"Today, we move from writing code to designing systems. You will learn the most important principle for organizing any complex project: the Single Responsibility Principle. By the end of this session, you will be able to transform a messy, monolithic script into a clean, modular, and professional system of interacting classes that is easy to test, reuse, and understand."**

---

## 2. Teaching Storyline

### Part 1: The "Why" - From Spaghetti to Structure (10 mins)

1.  **Start with a relatable problem.** "Who has ever written a data science script that started simple but grew into a 500-line monster? It's a file you're afraid to touch because changing one thing might break something else. This is 'spaghetti code'."
2.  **Show the monolithic script example.** Walk through the `titanic` script in the notebook. Point out the different sections commented with `# 1. Load Data`, `# 2. Preprocess`, etc. "This works, but it's fragile and inflexible. What if we want to reuse the preprocessing on new data? Copy-paste. What if we want to try a different model? Hunt for the right lines and carefully replace them. This isn't sustainable."
3.  **Introduce the core idea.** "The solution is to stop thinking of our project as one long recipe and start thinking of it as a team of specialists. Each specialist has exactly one job. In programming, this idea is called the **Single Responsibility Principle**."

### Part 2: The "What" - Designing a System of Classes (25 mins)

1.  **Introduce the Single Responsibility Principle (SRP).** "A class should have only one reason to change."
    *   Use a real-world analogy: "In a restaurant, you have a chef, a waiter, and a dishwasher. The chef's job is to cook. If the menu changes, the chef's job changes. The waiter's job is to take orders and serve. If the table layout changes, the waiter's job changes. You don't have one person who does everything. It's inefficient and chaotic. SRP is about creating these 'specialist' classes."
2.  **Design the classes for the Titanic script.** "Let's be the architects. What are the distinct jobs in our script?" Guide the students to identify the roles:
    *   `DataLoader` (Job: loading data)
    *   `DataPreprocessor` (Job: cleaning data)
    *   `ModelTrainer` (Job: training a model)
    *   `Pipeline` (Job: managing the whole process)
3.  **Implement the specialist classes.** Go through the notebook and show the implementation for `DataLoader`, `DataPreprocessor`, and `ModelTrainer`. For each class, explicitly state its single responsibility. "Notice how simple `DataLoader` is. It doesn't know about cleaning. It does one thing well. `DataPreprocessor` only knows how to clean. It doesn't know where the data came from or what model will be used."
4.  **Introduce Composition ("Has-A").** "Now, how do we connect them? We need a manager, our `Pipeline`. Does the `Pipeline` *become* a `DataLoader`? No. Does it *become* a `ModelTrainer`? No. The `Pipeline` *has a* `DataLoader` and it *has a* `ModelTrainer`. This is **Composition**."
    *   Contrast with Inheritance: "A `GoldenRetriever` **is a** `Dog` (Inheritance). A `Car` **has an** `Engine` (Composition). Favoring composition leads to more flexible systems."
5.  **Implement the `Pipeline` using Dependency Injection.**
    *   Code the `Pipeline` class. Point directly to the `__init__` method: `__init__(self, data_loader, preprocessor, model_trainer)`.
    *   "This is a crucial pattern called **Dependency Injection**. The pipeline doesn't create its own specialists; they are *given* to it. It's like hiring a team of contractors. This makes our pipeline incredibly flexible. It doesn't care *which* data loader you give it, as long as it has a `.load()` method."
    *   Code the `run()` method, showing how it delegates tasks: `self.data_loader.load()`, `self.preprocessor.process(...)`, etc. "The `run` method reads like a high-level project plan. It's the story of our workflow."

### Part 3: The "So What" - The Payoff (10 mins)

1.  **Assemble and run the pipeline.** Go to the "Putting It All Together" section. "Look how clean our main script is now. We have a configuration block, an assembly block where we build our team, and an execution block that is just one line: `pipeline.run()`."
2.  **Demonstrate the flexibility.** This is the most important part of the lecture.
    *   "What if we want to try a different model? Do we change the pipeline? No. Do we change the preprocessor? No."
    *   Scroll to the `RandomForestClassifier` example. "We just create a *new* `ModelTrainer` with a different model inside it, and plug it into our pipeline. Everything else stays the same. We just swapped one specialist for another."
    *   This is the "Aha!" moment that proves the value of the design.
3.  **Recap the benefits.** Reiterate the points: Readability, Reusability, Testability, Flexibility. "You are no longer just a coder; you are a system architect."

### Part 4: The Bridge to Next Time (5 mins)

1.  **Summarize the journey.** "We've gone from a single messy script to a well-organized system of collaborating objects. We used the Single Responsibility Principle to define our classes and Composition to assemble them into a flexible and powerful pipeline."
2.  **Set the stage for the final case study.** "You now have all the tools. You know how to create classes, how to use inheritance and polymorphism, and how to structure a project with multiple classes. In our next and final lesson, we will put it all together. We will tackle a complete machine learning problem from scratch, designing and building an end-to-end pipeline that solidifies every concept we've learned in this course."
3.  **Introduce the practice notebook.** "The exercises will give you a chance to practice being an architect. You'll identify responsibilities in messy code and build your own small, well-structured pipelines using composition."

---

## 3. What to Emphasize in Simple Language

*   **"One job per class. That's it. That's the secret."**
*   **"Think of your main `Pipeline` or `Processor` class as the manager. The other classes are the employees it delegates tasks to."**
*   **"Inheritance is for 'is-a' (a `Cat` is an `Animal`). Composition is for 'has-a' (a `Car` has an `Engine`). You will use 'has-a' far more often."**
*   **"Don't create your dependencies, inject them. Pass them into the `__init__` method."**

---

## 4. Where Beginners Usually Get Confused

*   **Knowing how many classes to create.** They might over- or under-abstract. Guide them by asking, "If you had to change *only* the data source, what code would you touch? That's one class. If you had to change *only* the model, what code would you touch? That's another class."
*   **Composition vs. Inheritance.** The "is-a" vs. "has-a" test is the best tool. Use the `Car`/`Engine` and `Dog`/`Animal` examples repeatedly. They are very intuitive.
*   **Dependency Injection.** The term sounds intimidating. Avoid using it at first. Just say, "We pass the objects the pipeline needs into its `__init__` method." After they see it in action, you can name the pattern: "This pattern of giving a class its dependencies is called Dependency Injection."

---

## 5. Demo Flow for Notebook Walkthrough

1.  Run the "spaghetti code" cell. Acknowledge that it works, but point out its fragility.
2.  Scroll down to the class definitions. Explain the single responsibility of each one as you go.
3.  Show the `Pipeline` class, emphasizing the `__init__` method (dependency injection) and the `run` method (delegation).
4.  Run the "Assembly" and "Execution" cells. The output should be identical to the first script, proving the refactoring worked.
5.  **The big finale:** Run the `RandomForestClassifier` cell. Show that you get a different accuracy score with minimal code changes. This is the proof of the design's power.

---

## 6. Strong Closing Summary

"Today, you learned the difference between a script and a system. A script is a set of instructions. A system is a set of collaborating components. By using the Single Responsibility Principle and Composition, you can build systems that are robust, flexible, and professional. This is the most critical skill that separates junior programmers from senior architects, and you now have the foundation to build your data science projects in this clean, scalable way."
