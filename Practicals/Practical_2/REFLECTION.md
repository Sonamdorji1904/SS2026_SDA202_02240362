# Personal Reflection: APAGS System Modeling

## Overview
In this practical, I focused on designing a robust architectural foundation for the **Automated Grading System (AGS)**.  
Before starting this practical I thought of moving beyond a simple "flowchart" mindset and apply formal **UML (Unified Modeling Language)** principles to capture the complex interactions between students, instructors, and external services.


## Applying Conceptual Logic to Formal UML
The most critical part of my learning process was understanding the hierarchy of diagrams.  

- I started with an Interaction Overview Diagram (IOD) to map the high-level control flow.  
- I learned that in a formal IOD, I should use **ref (Interaction Use)** blocks to represent complex sub-processes.  
- This allows the overview to stay clean while signaling that a more detailed Sequence Diagram exists for specific actions like *Plagiarism Detection* or *Automated Testing*.  

When I moved to the Use Case Diagram (UCD), I applied the concepts of **«include»** and **«extend»** to define functional requirements:

- I used **«include»** for mandatory steps like validation and testing, which must occur for every submission.  
- I applied **«extend»** for the *Flag Suspicious Code* use case, recognizing that this is a conditional outcome that only triggers if the TurnItIn similarity score exceeds a certain threshold.


## Refining the Interaction Flow
In the Sequence Diagram, I learned to balance technical detail with readability.  

- The initial draft browsed from AI was overly cluttered with internal system messages  
- so I tried to apply a *"simpler but not too simple"* approach by:

  - **Consolidating Participants:**  
    Grouping internal system modules into a single AGS boundary.

  - **External Integration:**  
    Explicitly showing the handshakes with TurnItIn and the LMS, which are essential for the system to achieve its final outcome.

  - **The Lifecycle Loop:**  
    Implementing a loop for student submissions and an `opt` block for instructor overrides, ensuring the diagram covered the entire lifecycle from assignment setup to final grade publication.


## Technical Challenges and Tooling
A significant part of this practical involved experimenting with PlantUML and Mermaid.  

- While these **"Diagrams as Code"** tools are excellent for technical accuracy  
- but, I found it challenging to keep the diagrams visually simple and intuitive while using only code.  
- The diagrams felt too rigid or cluttered.  

To overcome this, I tried a hybrid workflow:

- I used PlantUML/Mermaid to generate the technically correct **skeleton** of the diagrams (the boxes, relationships, and logic).  
- I drew manually those diagrams in Excalidraw to have a clean and understandable diagram.


## Conclusion
By completing these practical (three diagrams), I have learned that system design is not just about drawing boxes and arrows; it is about defining the functional boundaries and responsibilities of the system. I now have a clear understanding of how the system supports the actors to achieve the final outcome of a validated, graded, and audited programming assignment.