# Coding Conventions

When it comes to writing clean and quality code in C#, there are conventions developers should follow to make this possible. Here are some of the best coding practices every C# developer should know:

**Naming conventions:**

* Use PascalCasing for class names and method names.
* Use CamelCase for variable names, with the first letter in lowercase.
* Avoid using Hungarian notation to name variables (it’s outdated) and don’t use variable names that resemble keywords .
* Only use pascal case when naming classes and methods. Camel case should be used for variables and method parameters.

**Layout conventions:**

* Use formatting to emphasize the structure of your code and to make the code easier to read .
* Use the default Code Editor settings (smart indenting, four-character indents, tabs saved as spaces).
* If continuation lines are not indented automatically, indent them one tab stop (four spaces).
* Add at least one blank line between method definitions and property definitions.
* Use parentheses to make clauses in an expression apparent.

**Using variables and parameters:**

* Use predefined data types over system data types.
* Only use `out` when really needed in interop type scenarios. In all other cases, simply do not use `out`.
* Do place all `out` parameters after all of the pass-by-value and `ref` parameters (excluding parameter arrays), even if this results in an inconsistency in parameter ordering between overloads. This convention makes the method signature easier to understand.

**General programming practices:**

* Use a tested and certified C# coding library instead of creating your entire coding library from scratch.
* Establish a code style convention because it ensures consistency and readability.
* Use conditional attributes when necessary.
* Develop software application and class libraries in .NET using C# as a language.
* Use proper naming conventions and commenting conversions.
* Use a Quality IDE (IDE) such as Visual Studio or Rider IDE, implement a version control.
* Follow Microsoft's C# Coding Conventions and, where applicable, Microsoft's Secure Coding Guidelines.

By following these guidelines, you will be gaining the knowledge of experienced C# developers and will be able to avoid making some mistakes.
