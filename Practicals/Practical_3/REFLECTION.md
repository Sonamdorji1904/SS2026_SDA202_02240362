## UML Class Diagram & Object Diagrams

In this practical, I got to know the difference between **Class diagram** and its **Object diagram**.

### Distinguishing the Class Digram from the Object Diagram
The most significant realization during this practical was the conceptual distinction between Class and Object diagrams, which at first glance, can appear identical:

* **The Class Diagram:** I designed this to act as the permanent "DNA" or blueprint of the AGS. It defines the rules—such as how an `Assignment` **aggregates** multiple `Submissions`. It describes what a `Student` *is* in a general sense.
* **The Object Diagram:** This provided the "snapshot" of the system. I learned that while a Class Diagram shows the potential for data, the Object Diagram shows the **actual data** at a specific moment in time (e.g., showing that `ali : Student` has a specific `studentId` of `"S001"`).

### Navigating Technical Constraints
Applying the "Diagrams as Code" philosophy to these structural models presented unique technical hurdles that refined my troubleshooting skills:

* **Syntax Sensitivity:** I encountered a "Syntax Error" when attempting to import my Mermaid code into Excalidraw. This was a valuable lesson in tool-specific constraints; the parser struggled with the `instance : Class` naming convention within quotes.
* **Refining the Logic:** To fix this, I had to simplify the identifiers. I learned that while Mermaid uses the `classDiagram` syntax for both, the difference lies in the **intent**: removing multiplicity (the `1..*` symbols) and replacing variable types with **concrete values** (e.g., changing `score : float` to `score = 87.5`).

### Hybrid Workflow Execution
Consistent with my previous diagrams, I utilized a hybrid workflow to balance technical accuracy with visual clarity:

1.  **Skeleton Generation:** I used Mermaid to generate the logical skeleton of the associations and composition links.
2.  **Excalidraw Refinement:** After importing the "clean" code, I used Excalidraw to manually format the headers to follow formal UML instance naming. This ensured that a human reader could immediately identify the diagram as a specific execution instance rather than a generic class structure.

## Updated Conclusion
Refining these structural diagrams has closed the gap in my design process. I now understand that a complete UML suite must address both **how the system behaves** (Sequence/Use Case) and **how the system is organized** (Class/Object). By documenting both the blueprints and a specific snapshot of those blueprints in action, I have created a design that is both technically robust for developers and contextually clear for system auditors.