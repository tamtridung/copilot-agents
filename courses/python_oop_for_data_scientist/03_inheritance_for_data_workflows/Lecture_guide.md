# Lecture Guide: Inheritance for Data Workflows

## 1. Lesson Promise

**"Today, you will learn the most powerful code-reuse technique in programming: inheritance. By the end of this session, you will be able to build a hierarchy of classes, just like in Scikit-learn, where specialized child classes inherit and extend the functionality of a general parent class, saving you from ever having to copy-paste code again."**

---

## 2. Teaching Storyline

### Part 1: The "Why" - The Sin of Copy-Pasting (10 mins)

1.  **Present a clear, relatable problem.** "In our last lesson, we built a `DataProcessor`. Now, imagine your boss asks you to handle two new data sources: one from a CSV file and one from a JSON file. The core logic is similar, but the loading part is different. What's the easiest, most tempting way to do this?"
2.  **Show the 'bad' way.** Quickly sketch out two separate classes, `CsvProcessor` and `JsonProcessor`, with a lot of copy-pasted code (e.g., methods for logging, tracking status, etc.). Then ask: "What happens if we find a bug in the logging method? We have to fix it in both places. What if we want to add a new feature, like a `get_status` method? We have to add it to both places. This is not scalable and is a recipe for bugs."
3.  **Introduce the solution: Inheritance.** "Object-Oriented Programming gives us a beautiful solution to this. It's called **Inheritance**. We can create a 'parent' class that contains all the shared code. Then, we can create 'child' classes that automatically get all of the parent's functionality for free. They only need to add or change the parts that are unique to them."
4.  **Use the "is-a" analogy.** "The key to knowing when to use inheritance is the **'is-a'** test. A `CsvProcessor` **is a** type of `DataProcessor`. A `JsonProcessor` **is a** type of `DataProcessor`. This tells us inheritance is the right tool. A `Car` **is a** `Vehicle`. A `Dog` **is an** `Animal`. This pattern is everywhere."

### Part 2: The "What" - Building a Class Family Tree (25 mins)

1.  **Transition to the `DataLoader` notebook.** "Let's build this system the right way. We'll start with a generic, abstract parent class."
2.  **Code the `DataLoader` (Parent Class).**
    *   Define the class. In the `__init__`, include common attributes like `source` and `_load_time`.
    *   Define a common, useful method that all children will want: `get_load_time()`.
    *   **Introduce `NotImplementedError`.** In the `load_data` method, `raise NotImplementedError`. Explain this concept clearly: "This is the parent class telling its future children: 'I'm not going to tell you *how* to load data, that's your job. But I am telling you that you *must* have a `load_data` method.' It's defining a contract."
3.  **Code the `CsvLoader` (Child Class).**
    *   **Show the inheritance syntax:** `class CsvLoader(DataLoader):`. Emphasize the parentheses.
    *   **Override the `load_data` method.** Provide the specific implementation for reading a CSV with Pandas.
    *   **Instantiate and use the child.** Create `my_csv_loader = CsvLoader(...)`.
    *   Call `my_csv_loader.load_data()`. It works.
    *   **The "Aha!" Moment:** Call `my_csv_loader.get_load_time()`. It also works! Pause here. "We never wrote `get_load_time` in our `CsvLoader` class. Where did it come from? It was inherited from the `DataLoader` parent. This is free functionality."
4.  **Introduce `super()` for extending `__init__`.**
    *   Pose the next problem: "What if our `CsvLoader` needs a special attribute, like the separator character?"
    *   Create the `CsvLoaderWithSeparator` class. Add a new `__init__` that takes `source` and `separator`.
    *   **Show the problem:** Explain that defining `__init__` in the child *replaces* the parent's `__init__`. The parent's setup code (`self.source = source`) no longer runs.
    *   **Introduce the solution:** `super().__init__(source)`. Explain it: "`super()` is a magic word that gives you a reference to the parent class. This line says, 'Hey, go find my parent and run *its* `__init__` method first, to do all the common setup. Then I'll do my own specific stuff.'"
    *   Walk through the execution flow of the `__init__` call, showing how both the parent and child constructors are now being called in order.

### Part 3: The "So What" - Real-World Connection (15 mins)

1.  **Connect to Scikit-learn.** This is the most important connection for data scientists. "Have you ever wondered why every single model in Scikit-learn, from `LinearRegression` to `RandomForestClassifier`, has the same `.fit()`, `.predict()` methods? It's because they all inherit from a common parent class called `BaseEstimator`. Scikit-learn's creators defined a contract, and every new model must follow it. When you make your own custom transformers, you do the exact same thing, inheriting from `TransformerMixin`."
2.  **Demonstrate the power of the pattern.** Quickly code the `JsonLoader` class. Point out how little code it requires. "Look how fast that was. We just provided the one piece that was different—the `load_data` implementation—and we got all the other functionality for free."
3.  **Discuss "Is-a" vs. "Has-a" (Composition).** Bring up the "Common Mistakes" section. "Inheritance is for an 'is-a' relationship. What about a `Car` and an `Engine`? A car *is not* an engine, a car *has an* engine. This is a different relationship called Composition, where you store an object as an attribute (`self.engine = Engine()`). Knowing the difference between 'is-a' and 'has-a' is key to good OOP design."

### Part 4: The Bridge to Next Time (5 mins)

1.  **Recap.** "Today we learned how to stop duplicating code. By creating parent classes with shared logic and child classes with specialized logic, we can build flexible and maintainable systems. We learned that `super()` is the glue that connects the child's constructor back to the parent's."
2.  **Set the stage.** "We now have these different loader objects: `CsvLoader`, `JsonLoader`. Right now, we have to know which type of object we have. But what if we could write a function that takes *any* kind of `DataLoader` and just calls `.load_data()` on it, without caring if it's a CSV or JSON loader? This powerful idea of treating different objects as if they are the same is called **Polymorphism**, and it's the final pillar of OOP that we'll explore next."
3.  **Introduce the practice notebook.** "The exercises for this lesson will have you build your own class hierarchies. Pay close attention to when you need to override methods and when you need to use `super()`."

---

## 3. What to Emphasize in Simple Language

*   **"Inheritance is 'buy, don't build'. You get all the parent's features for free."**
*   **"`super()` is like saying, 'Hey Mom/Dad, can you do your part of the setup first?'"**
*   **"If you can say 'X is a Y', you should probably use inheritance."**
*   **"Overriding is the child class saying, 'I know you taught me how to do this, but I have my own special way of doing it.'"**

---

## 4. Where Beginners Usually Get Confused

*   **`super()` is magic.** They don't know what it does or when to use it. The rule of thumb is simple: "If you write an `__init__` in a child class, the first line should probably be a call to `super().__init__()`."
*   **Forgetting to pass arguments to `super().__init__`**. They write `super().__init__()` but forget `super().__init__(parent_arg1, parent_arg2)`.
*   **Inheritance vs. Composition ("is-a" vs. "has-a").** The `Car` and `Engine` example is a good way to clarify this. It's a design choice, not a syntax one.
*   **The concept of an abstract base class** (like our first `DataLoader` with `NotImplementedError`). They might wonder why you'd create a class that you can't fully use. Explain that its purpose is not to be used directly, but to serve as a blueprint and a contract for its children.

---

## 5. Demo Flow for Notebook Walkthrough

1.  Run the `DataLoader` class definition.
2.  Run the `CsvLoader` definition.
3.  Run the `CsvLoader` usage cell. After calling `load_data`, pause and ask the class where `get_load_time` came from before running that line.
4.  Run the `CsvLoaderWithSeparator` definition. Step through the `__init__` method, explaining the call to `super()` and then the handling of `self.separator`.
5.  Run the usage cell for `CsvLoaderWithSeparator` to show it works.
6.  Quickly run the `JsonLoader` cells to demonstrate how easy it is to extend the system once the parent class is defined.
7.  Talk through the Scikit-learn example conceptually.

---

## 6. Strong Closing Summary

"Inheritance is a fundamental concept for writing clean, professional code. It allows us to build on the work we've already done, creating logical family trees of classes where each child is a more specialized version of its parent. By defining common interfaces in a base class and reusing code, we create systems that are easier to maintain, extend, and debug. You now have the tools to structure your data science projects like the pros who built Scikit-learn."
