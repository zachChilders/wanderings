# Going Pro: Oxidize Your Job Into A Rust Job

## Intro

If you're reading this, you probably already know what Rust is.  You know it's benefits, you've probably used it, and you may evenconsider yourself to be a Rustacean.  

However, Rust is still perceieved as a fairly young language with a lot to prove.  The job market for Rust jobs is still slim and while you may love writing Rust in your spare time, it isn't quite putting food on the table for most people.

I got frustrated with this situation.  Rust is clearly a strong up and coming ecosystem and will be around for a long time.  I wanted to shed my day-job language and work full time on Rust but am fortunate enough to have a secure job at a well established company.  It seemed like it would be a tradeoff between writing fragile but traditional C++ and enjoying writing secure, performant code.  

After spending some time looking outward to find Rust, I decided to take a hard look inward and see what I could do to make my _current_ job into a Rust job.  Here is some of the process I went through should you decide to embark on a similar journey.

## Is it Smart?

As much as we like to think that Rust is a silver bullet solution it doesn't always make sense for your project.  It's easy enough to start a new greenfield project in a new language, but that's almost never the case.

If you write in a large codebase, proposing a complete rewrite is often laughable.  Incremental change is almost always how succesful ports happen and definitely makes the difference between [Firefox's success](https://bholley.net/blog/2017/stylo.html) and [Netscape's failure](https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/).  The long road to software failures is paved with ambitious rewrites. 

In my situation, we had an extensive crossplatform and safe code story for high performance C++ clients.  We _still_ maintain a really extensive functional framework that's designed to handle cross platform system calls for us, and because of some regulatory requirements on our business some of it will never go away.

At its core though, some of our components exist as black boxes.  They have a relatively clean API between them and we ship them seperately.  The common framework we have used for years shares many of the design goals with rust in regards to compile-time memory safety and cross platform compatiblity, but it enforces it all through tricking clang into supporting these features instead of working with a compiler built from the ground up to support them.

For my situation, the state we started in was actually _really_ close to a desirable state to be in.  This brought the cost of a Rust PoC down significantly and helped prove to the right people that at least in our case, it was worth a risk.

## Technical Approach

Technically speaking, the problem of incremental rewrites is actually pretty close to being a solved problem.  Aggressive paradigm shifts in recent years have made many organizations more comforatable with the thought of a rewrite.  

Most organizations have gotten used to the thought of purposefully developing loosely coupled architectures that can be broken up and replaced piece by piece without terrible lasting consequences.  

Two related patterns in particular lend themselves well to a rewrite:

### Microservices

If you are in web services, this is probably the pattern you're going to need.  Microservices lend themselves extremely well to rewrites because, well, there's a lot of them.  You can choose a single service, do a proof of concept rewrite, and test it out. 
 
You have a definitive end goal and means to acheive it, because the entire point of Microservices is to isolate functionality into hot-swappable components.

If you have done a transition from monolithic services to microservices, you probably know what to do already.  The name of the game is to find the highest impact component to rewrite for the lowest cost.

You want to find something that you can reimplement:
- with high confidence you did it right (have integration tests)
- with minimal external dependencies
- without bringing everything else
- inside of a week

#### Challenges

### Strangler

#### General Approach
#### Challenges

## People Approach

### Team Allies

### Pros/Cons

### Propaganda
