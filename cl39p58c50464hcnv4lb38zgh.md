## Software Design - Modules Should Be Deep

There is a common sentiment among software developers that modules should be small. In OO (object oriented) programming, we can apply this to classes, but a module can refer also to sub-systems and entire services. Indeed, the trend of building things as a collection of microservices is a logical parallel to draw. When we talk about microservices, we often talk about the things you trade off when implementing them. You trade simplicity in the individual parts for greater complexity in the whole, among other things. This is not a line of thinking I often see when we are talking about the building blocks of our systems, the classes and functions.


Chapter 4 of [A Philosophy of Software Design by John Ousterhout](https://www.goodreads.com/book/show/39996759-a-philosophy-of-software-design) focuses on how modules should present a simple interface to complex behavior, considering it in terms of cost versus benefit. Any abstraction added to a system has a cost associated with it, and the cost relates to the complexity of the interface it presents. Abstractions can become a net-negative if the interface they present is of equal or greater complexity than the behaviour they provide.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641607891801/J4yvQSVc6Q.png)


Well-constructed systems tend to favour fewer, simpler abstractions providing a gateway to rich functionality.


---


While reading this chapter I couldn't help but draw a comparison with Clean Code. The two books use different terminology -- Clean Code is framed around OO programming -- but Uncle Bob states:


> The first rule of classes is that they should be small. The second rule of classes is that they should be smaller than that.

> ...as with functions, smaller is the primary rule when it comes to designing classes.

This focus on size is where the Clean Code philosophy really falls apart for me. Size is an indication that we should take a closer look, nothing more, nothing less. Cohesion and levels of abstraction should be the rule here. If I have a class that is over 100 lines long, but it has strong cohesion, and a consistent level of abstraction, should I feel compelled to break it up _just_ because it has breached an arbitrary upper limit on length? Clean Code seems to suggest that I should.


A small class, by design, is not going to provide rich behaviour in and of itself. It will probably delegate to some other classes providing their own small slice of the high-level function. There is a strict adherence to this rule of size, as well as a strong desire for _everything_ to be made into a class no matter how clunky the result becomes.
He provides an analogy:

> 
Do you want your tools organized into toolboxes with many small drawers each containing well-defined and well-labeled components? Or do you want a few drawers that you just toss everything into?


There is a bias here, he wouldn't have written the book if he didn't feel strongly about these ideas. However, the suggestion that you either work in the way he describes, otherwise you are creating a dumping ground mess of a module, is something I can't agree with.


It's also a simplistic view of the complexity in our systems. It's unclear exactly what he is analogising to the toolboxes and drawers in the quote above, presumably he's talking about classes in isolation. Having more and more toolboxes with again more drawers for increasingly specific purposes doesn't automatically make a more organised workshop, no matter how well labeled they are. To continue to torture the metaphor, it's sort of like having a drawer for every different type of screw, and each individual screw also in its own drawer. At some point, continuing to break things down not only doesn't make things clearer, but it actually slows you down while you're sitting there opening drawers.... wait, what was I talking about?


An alternative heuristic for class design and introducing abstractions could be:

> 
💡 Does this abstraction add more value/reduce complexity of the system such that its introduction outweighs any added complexity for its own existence?


Not quite as succinct as "classes should be small", but it has more utility this way. It's open for interpretation, because code is such a creative and subjective medium. Notice there is no mention of length or lines of code or anything like that. Within reason, function or class length is immaterial to complexity of a system. Here I make a another statement that is core to this idea: complexity in the system is more detrimental than complexity in individual components. All other things being equal, I would much prefer fewer more complex components than many more increasingly abstracted components. 


So we weigh the introduction of a new abstraction against its complexity cost to the rest of the system. We prefer OO code to procedural where it makes sense, but we don't _force_ our code into classes where it doesn't.



---


As with so many things in software, there is a tradeoff that we must consider when adding a new abstraction. The tradeoff is around overall clarity of the system versus the complexity cost incurred for introducing this abstraction. More is not better in this case, and so we should be very picky about when and how we introduce new abstractions to our software.