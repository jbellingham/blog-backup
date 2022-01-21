## My Journey Towards Cleaner Code - Part 2

In my  [last post](https://blog.jessebellingham.com/my-journey-towards-cleaner-code) I wrote about my first steps on this cleaner code journey. I was recommended the book Clean Code by Uncle Bob, and it was like a sort of revelation, I had never thought about writing code like this and it all made so much sense. I became _very_ enthusiastic about the Clean Code way of writing, and on occasion would venture beyond the safety of my team to talk about it.

As silly as it now seems to me, I sort of just thought everyone would agree with me, that this is what good code looked like, and we should be convincing everyone to write like this. I still remember the conversation that clued me into this not being the case, and I remember feeling like a bit of a dummy, but its all part of the journey right?

So anyway, code is subjective, and what _good_ code looks like is even more subjective...

### A New Perspective

I don't quite remember how I found it, but I stumbled upon [this critique](https://qntm.org/clean) of Clean Code. In it, the author goes into a fair amount of detail, breaking down a number of the key ideas from the book, and arguing against them. There are even a handful of flat out contradictions in the book pointed out that I hadn't noticed while reading. As silly as it now sounds to me, I think reading this critique was probably the first time I really stopped to question the ideas presented in Clean Code. In my opinion, the author presents a number of compelling arguments against Uncle Bob's ideas, and it got me interested in reading more different perspectives.

A book that comes up as a recommendation several times in the comments section of that post is [A Philosophy of Software Design by John Ousterhout](https://www.goodreads.com/book/show/39996759-a-philosophy-of-software-design).  It is comparatively short (190 pages to Clean Code's 464), so I actually _finished_ it. Its brevity means it is necessarily light on details; it is on the whole, more interested in the design of software at a high-level, than intense scrutiny of code line-by-line. For this reason, it's probably not a book I would recommend to anyone with less than a solid 2 or 3 years of programming experience. Not because the ideas are difficult to comprehend; actually the book is very readable in that sense; but rather because this book works best when you have a decent bank of experience working in messy code to compare with.


![Clean](https://cdn.hashnode.com/res/hashnode/image/upload/v1642502064801/asgEw9rjy.jpeg)
> Photo by <a href="https://unsplash.com/@jeshoots?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">JESHOOTS.COM</a> on <a href="https://unsplash.com/s/photos/clean?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
  

Where Clean Code now feels a little _too_ prescriptive, and very focused on the minutiae, A Philosophy... presents its views without describing in excruciating detail exactly what the code should actually look like. There is perhaps an assumption here that the reader has enough experience to know how to name a variable, or to probably not put that 1000 lines of code into one function, or whatever.

The lack of focus on those small details suggests that maybe in the grand scheme of things they are less impactful to the overall complexity of a system. Not that they're not important, but rather, they're not _so_ important that we need a chapter telling you precisely what to write and what not to write.

I actually think the two books make a pretty good pairing, as you can read Clean Code, and see the examples in all their specificity, and then see how A Philosophy... provides a sort of counterpoint to the examples. Overall, I really enjoyed A Philosophy...; I find myself more in agreement with the views presented there, especially as I work in more legacy codebases and see for myself some of the anti-patterns and red flags the book talks about. I think if someone was to ask me for a recommendation of a book that captures the essence of good software design, this would be it.

---

This was another pretty short post, but in the next one I'll be looking more closely at how we build modules in software, and comparing ideas on the subject from A Philosophy... and Clean Code.

