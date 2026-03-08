# Lecture Guide: Polymorphism in Data Science

## 1. Lesson Promise

**"Today, you will learn how to write truly flexible code that adapts to different types of objects. By the end of this session, you will understand polymorphism and be able to write a single function that can process different kinds of data loaders or models, just like a Scikit-learn Pipeline handles different transformers."**

---

## 2. Teaching Storyline

### Part 1: The "Why" - The Ugly `if/elif` Chain (10 mins)

1.  **Start with the problem.** "We've built our `CsvLoader` and `JsonLoader`. Now, let's say we have a list containing a mix of these loader objects. How do we process them all? The most obvious way is to check the type of each one."
2.  **Show the 'bad' code.** Write a loop that uses `isinstance()`:
    ```python
    for loader in loaders:
        if isinstance(loader, CsvLoader):
            # do CSV stuff
        elif isinstance(loader, JsonLoader):
            # do JSON stuff
    ```
3.  **Critique the bad code.** "This works, but it's terrible. Why? Because every time we invent a new loader, like an `ExcelLoader`, we have to come back and *modify* this function. Our function is not 'closed for modification'. It's brittle and hard to maintain. This is a classic sign of poorly designed code."
4.  **Introduce the solution: Polymorphism.** "The elegant solution is **Polymorphism**, which literally means 'many forms'. It's the ability to write a single, generic function that can work with objects of many different classes, without ever needing to know what those classes are."
5.  **Use the Power Outlet Analogy.** "Think of a wall outlet. It's a single, standard interface. You can plug in a lamp, a phone charger, or a vacuum. The outlet doesn't care what the device is; it just provides power. The device knows what to do with that power. Polymorphism lets us build the 'wall outlet' for our code."

### Part 2: The "What" - Duck Typing and Shared Interfaces (20 mins)

1.  **Explain Python's approach: Duck Typing.** "In many languages, polymorphism is strictly tied to inheritance. In Python, it's more flexible. We have a principle called **Duck Typing**: 'If it walks like a duck and quacks like a duck, it must be a duck.' This means Python doesn't care about an object's class. It only cares if the object has the method you're trying to call."
2.  **Transition to the `DataLoader` notebook.** "Our `DataLoader` classes are perfect for this. They all 'quack' in the same way: they all have a `load_data()` method."
3.  **Code the polymorphic function.**
    *   Write the `process_and_summarize(loader)` function. Emphasize that the type hint `loader: DataLoader` is a suggestion; the function will work with *any* object that has a `.load_data()` method.
    *   Inside the function, make the polymorphic call: `df = loader.load_data()`. This is the key line. "At this moment, we don't know if we're running the CSV version or the JSON version. We don't care. We just trust the object to do the right thing."
4.  **Demonstrate it in action.**
    *   Create a list of different loader objects (`CsvLoader`, `JsonLoader`, `ExcelLoader`).
    *   Loop through the list and pass each object to the *same* `process_and_summarize` function.
    *   Run the cell. Pause as the output appears, pointing out how the `--- Using CSV/JSON/Excel Loader ---` messages show that different code is being executed for each object, even though the calling function is identical. This is the "Aha!" moment for polymorphism.
5.  **Reinforce the flexibility.** "What if we invent a `ParquetLoader` tomorrow? Do we need to change our `process_and_summarize` function? No! As long as `ParquetLoader` has a `load_data` method, it will work instantly. Our function is now open for extension but closed for modification."

### Part 3: The "So What" - Polymorphism in the Wild (15 mins)

1.  **Connect to Scikit-learn Pipelines.** This is the killer example. "Where have you seen this before? Every day, in Scikit-learn. A `Pipeline` is a list of steps. Each step can be a `StandardScaler`, a `PCA`, a `SimpleImputer`, or your own custom class. When you call `pipeline.fit_transform()`, how does it work? It doesn't have a giant `if/elif` chain. It's polymorphic! It simply loops through its steps and calls `.fit_transform()` on each one, trusting that each object has that method."
2.  **Live-code the mini-pipeline simulation.**
    *   Create the simple `MeanRemover` and `Scaler` classes. Point out that they are not related by inheritance, but they share the `fit_transform` interface.
    *   Write the `run_pipeline` function. Again, highlight the polymorphic call: `temp_data = transformer.fit_transform(temp_data)`.
    *   Execute the pipeline and show the result.
3.  **Discuss the "Common Mistakes".**
    *   **The `isinstance` anti-pattern:** Reiterate that if you're checking the type to decide what method to call, you're missing the point of polymorphism.
    *   **Inconsistent method signatures:** "What if one transformer's `fit_transform` needed an extra argument? The shared interface would be broken, and our simple polymorphic loop would fail. The 'plugs' have to have the same shape."

### Part 4: The Bridge to Next Time (5 mins)

1.  **Recap.** "Polymorphism is what makes object-oriented code so flexible and powerful. By relying on shared interfaces—what we call 'duck typing'—we can write simple, clean functions that work on an infinite variety of objects. We've seen how this pattern is the engine behind powerful tools like Scikit-learn's Pipeline."
2.  **Set the stage.** "We've now covered the three pillars of OOP: Encapsulation, Inheritance, and Polymorphism. You have the core tools for building professional-grade systems. But to make our custom objects feel truly integrated and 'Pythonic', we need to learn one more thing: how to hook into Python's own internal behaviors. We can do this with special methods like `__len__`, `__str__`, and `__add__`. These are called **magic methods**, and they are the topic of our next lesson."
3.  **Introduce the practice notebook.** "The exercises for today are all about spotting and fixing code that isn't polymorphic. Your goal is to eliminate `if/isinstance` checks and replace them with clean, direct method calls."

---

## 3. What to Emphasize in Simple Language

*   **"Don't check for the type of the duck, just tell it to quack."**
*   **"Polymorphism is writing one function that works for many different types of objects."**
*   **"The `if/isinstance` ladder is a sign that you should be using polymorphism instead."**
*   **"Think of the interface as a contract. As long as your classes honor the contract (i.e., have the right methods), they can play the game."**

---

## 4. Where Beginners Usually Get Confused

*   **It feels too simple.** Polymorphism is more of a design pattern than a complex syntax, so they might feel like they're missing something. The "before" and "after" refactoring example (Question 4 in the practice notebook) is key to making the benefit concrete.
*   **The difference between polymorphism and inheritance.** Explain that inheritance is *one way* to achieve polymorphism (by guaranteeing a shared interface), but it's not the only way in Python (thanks to duck typing).
*   **Why `isinstance` is bad.** It's not always bad, but it's bad when used to control program flow based on type. Explain that it creates a maintenance burden.

---

## 5. Demo Flow for Notebook Walkthrough

1.  Run the cell defining the `DataLoader` and its children.
2.  Run the cell defining the polymorphic `process_and_summarize` function.
3.  Run the main usage cell. As the output streams, point to the different "Using X Loader" messages and emphasize that the *same function* is producing this varied behavior.
4.  Transition to the Scikit-learn pipeline example. Run the transformer definitions.
5.  Run the `run_pipeline` function and its usage cell. Explain how the data is being transformed at each step by a different object, all through the same `.fit_transform()` call.
6.  Briefly discuss the common mistakes, showing the "bad" `isinstance` pattern again to reinforce what to avoid.

---

## 6. Strong Closing Summary

"Today, you've learned the principle that separates rigid code from flexible code. Polymorphism, through Python's duck typing, allows us to build systems that are incredibly easy to extend. By focusing on what an object *can do* (its methods) rather than what it *is* (its class), we can write cleaner, simpler, and more powerful code. This is the design philosophy that makes libraries like Scikit-learn possible. You now have all the major pillars of OOP in your toolkit."
