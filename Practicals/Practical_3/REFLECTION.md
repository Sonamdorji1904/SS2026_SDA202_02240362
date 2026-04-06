## Practical 3: Class Diagram & Object Diagram

This part of the practical was a really challenging for me, especially regarding the difference between a **Class diagram** and an **Object diagram**. At first, they looked almost identical, and I wasn't sure why I needed to do both.

### What I Learned: Blueprints vs. Snapshots
I realized that the Class diagram is basically the "rules" of my Automated Grading System (AGS). It’s the blueprint that says every `Assignment` is allowed to have multiple `Submissions`. 

However, the Object diagram is much more specific, it’s like taking a screenshot of the system while it’s actually running. Instead of just seeing a generic "Student" class, I see a specific person, like `sonam : Student`, with their actual ID `Std01`. It helped me visualize how the data actually fills up the boxes I designed in the class diagram.

### Challenges Faced

The biggest hurdle during this practical wasn't just drawing the boxes, but correctly defining the **relationships** between them. I struggled with choosing the right types of arrows to represent how the system components actually connect.

* **Deciding on Arrow Types:** I found it challenging to distinguish between a simple association and a stronger bond like **Composition**. For example, I had doubt about whether a `Result` can exist without a `Submission`. I realized that since a grade is strictly tied to a specific piece of work, it should be a composition (the solid diamond arrow), because the result wouldn't exist if the submission were deleted.
* **Relationship Direction:** I initially got confused about which way the arrows should point.Later after going through the resources, I found out that the arrow usually points toward the object being acted upon or the "part" of the whole. 
* **Object Diagram Logic:** When moving to the Object Diagram, I realized I had to drop the "1 to many" (multiplicity) notations. 



### My Workflow
I used Mermaid to build the basic skeleton because it’s much faster than drawing and aligning boxes by hand. But once the structure was there, I moved it into Excalidraw to clean it up. This allowed me to make the diagram look clean and readable while ensuring the logic underneath was technically correct.

## Conclusion
Doing these two diagrams together really helped me understand the difference between them. The `Class Diagram` is actually the **blueprint** of any system and the `Object Diagram` is the **snapshot** meaning what actually it looks like while the system is running whcih gives teh whole image of how our system will work. I now have a clear understanding of how and what are the differences between **Class Diagram** and **Object Diagram**.