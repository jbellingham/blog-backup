## Spike And Stabilize

In agile, when enough people agree that there isn't enough information to start development on a new piece of work, we do a spike in order to (hopefully) better understand what needs to be done, and how we might do it.

> 
A **spike** in agile terms generally means:
- It's an experiment
- It's timeboxed
- Any code created gets thrown away at the end

There are other reasons to do a spike, like getting fast feedback on a new feature, or just seeing if something is technically possible. The reason for doing a spike isn't super relevant to this post, only the idea that a spike is usually borne out of _some_ kind of ambiguity.

---

In the stocks trading world there is the concept of an _option_. An option has a value, which changes situationally based on the fluctuations of the currencies it relates to. An option also has an expiry date. As the name sort of implies, an option is something you have a **right**, but not an **obligation** to fulfill. Common wisdom is to not commit an option early, unless you have a really good reason.

The choice to make something a spike or production code is an option. We make that choice right at the beginning, before we have any working code, essentially committing the option immediately without any obvious reason to do so. Why do we do this?

**An aside about consulting and uncertainty**

The longer I work as a developer, the more I find myself wanting to consider everything a spike. Especially now working as a consultant, it is my job to be perpetually working in codebases that I am largely unfamiliar with and trying to gain some feeling of confidence in my work. I don't think these thoughts are something to hide, it's not a sign of a bad developer. Quite the contrary, I think having the ability to express uncertainty about a piece of work is a sign of a confident developer who has seen the negative effects a "fake it 'til you make it" mindset can have.

With this in mind, is there a different way we can frame our work? A pattern or way of working we can employ that takes this uncertainty into account and fuels a more intentional and iterative design and development process.

### A new way of working

Thinking somewhat idealistically, our code should fit into one of two categories. It's either:

- Very new, **or**
- Very stable

Stable here has a particular meaning. Beyond simply functioning as expected, stable code has had a critical eye over it and represents (close to) the ideal state for that particular piece of code. It is well tested, well documented, and appropriately performant.

Code that falls outside of these two categories is where we start to have problems. It's no longer all that new, so a lot of the context we had while writing it is forgotten. It's also not particularly stable, maybe it has little or no test coverage; maybe it doesn't conform to an established pattern; for one reason or another, it just doesn't serve its purpose very well.

---

**Spike and Stabilize** is a pattern for deliberately deciding which code should go from young to old, new to stable.

**Spiking**

First, we say that **all code is a spike.** All code is an experiment. We create _okay_ code, but we deliberately cut corners. We use _TODOs_ to say, we know we should do things this way, but we just want to get the experiment out there. The experiment may not have amazing test coverage or documentation, but you deploy it anyway.

Some time later, you pair review the code, looking at all the TODOs. In the time since first writing it, there may be some TODOs that no longer apply for one reason or another, and so time spent addressing those TODOs at the beginning would have been wasted.

**Stabilizing**

When we come back this second time for review, this is when we pay down the quality. Now, rather than tests *driving* the code, we write **characterisation** tests, tests that describe how we _actually_ use the code. We realise that in order to test our code with more granularity, we need to move it around a bit. While we're writing tests and molding our code into the shape we want, it starts to look a lot like it was test-driven in the first place.

What we're basically doing here is very intentional [evolutionary design](https://www.martinfowler.com/articles/designDead.html#PlannedAndEvolutionaryDesign). By allowing some time to elapse between the first and second passes of a piece of work, we're allowing the team time to further contextualize the work. When it comes time to review a piece of work, there are a number of possible outcomes:
- The TODOs we left for ourselves are still relevant, so we take the time to address them now, with the benefit of hindsight.
- Some or all of the TODOs we left for ourselves are no longer relevant.
- Something has changed that has made this work redundant entirely, lucky we didn't spend twice as long or more doing it the _right_ way!
- With more context we discovered a better design for the code we added previously.

---

**Caveat**

This pattern is not an easy one to adopt. It requires a fair amount of discipline on the part of the devs participating for a couple of reasons:

1. They need to know what corners they can safely cut to get a spike into production.
2. They need to be able to effectively stabilize a piece of spiked code, which is often easier said than done.

Beyond the part the developers play in all of this, getting buy-in from managers to work in this way can be a challenge. We're effectively saying that we're going to ship functional but unpolished work quickly, with the intention of coming back to finish it at some pre-determined time in the future. I can definitely see a desire to just skip the part where we come back and finish things, and now we just have bad code in production, yayyy...

---

This will (hopefully) become part of a series on team dynamics and ways of working.
I'd be really interested to hear if anyone has heard of/applied this pattern successfully (or not).


For more information, and just a really interesting talk with other novel ways of working, check out  [this talk](https://www.youtube.com/watch?v=USc-yLHXNUg)  by Dan North.