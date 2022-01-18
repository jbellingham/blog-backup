## My Journey Towards Cleaner Code - Part 1

**Edit:**
I originally had this as one big post but I've decided to break it into a multi-part series. I'll be talking about my journey trying to come to my own conclusions about what "good" code looks like. I read/watch a lot, so my opinions change with some frequency...


**Edit edit:**
This is a repost, because Hashnode did something weird with the last publish of it.

---

The idea of there being some objective measure of whether code is _good_ or not, is one that I have unironically entertained before. Call it inexperience or misplaced optimism, but I truly thought that if I spent enough time reading about what other people think makes code good, that I would eventually be enlightened; I could simply apply what I've learned, and I would finally be an author of _good_ code myself.

Of course, there _is_ no objective measure of whether code is good or not. In fact, there are very few things that are universally agreed upon in software.

### Something journey... something something single step

One of the very first pieces of literature I read on this topic was Clean Code, by Robert C. Martin, and it was this book that sparked my interest and really got me down this path in the first place. The book was recommended to me by my mentor at the time, a person whom I still have a great deal of admiration for. I remember reading the first few chapters and feeling a genuine sense of excitement to go and apply the ideas I was reading about. So much of what Uncle Bob had written made complete sense to me, to the point that I wondered how I hadn't come to the same conclusions myself more organically. I became a bit of a Clean Code evangelist; I ran a Clean Code book club within my team, and would always try to apply the ideas in my own work and encourage those around me to do the same.

For those reading that aren't already familiar, Clean Code presents a view of code that favours small methods and classes; well-named, descriptive variables and methods; data abstraction and implementation hiding; and as few comments as can be gotten away with. Some of that probably sounds pretty reasonable, but it's when he gets into the details that some questions arise...


![well yes, but actually no](https://cdn.hashnode.com/res/hashnode/image/upload/v1641547469684/AnBwQGwOX.jpeg)

Reading back over my notes from Clean Code, I find myself having the above reaction to a lot of the things he says, or perhaps more like "well maybe, it depends". He says things like:

> 
The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that.

Which sort of seems like a bit of hyperbole just to get the point across... until you see his code. He seems to have made this idea a core part of his signature code style. There are plenty more snippets of wisdom just like this peppered throughout the first half of book; many of which I took at face value, and at the time felt made a lot of sense.

Now, this post is not intended to be a critique of Clean Code, there is plenty of that already on the internet by much better developers than me. I actually quite like the book, even if these days its mostly just for setting me on this path. In truth though, I never even finished reading it, and I don't know if I ever will. I will make no attempt at any kind of _review_ of Clean Code, as that's not of any interest to me. I will however, be holding it up for comparison, mostly because it is my reference point.

In the next post in this series, I will be taking a look at another book that has been influential to me on this path.