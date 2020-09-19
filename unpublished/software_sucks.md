+++
title = "Security Software is Bullshit" 
+++

Odds are pretty high that I have untested code running on your computer right now.  I'm reasonably sure it works, or else somebody would have yelled at me about it by now.  I know this sounds awful, but how much worse is it than any other software you run?

Modern software development is really optimized around one metric: "engineering velocity".  This is a rough metric of how long it takes for new code to be written, tested, checked in, and deployed to customers.  There are a LOT of tools set up specifically for this, and entire industries devoted to increasing this velocity in various ways.  In general, velocity is good!  It makes our lives easier and in many ways, we're living in a golden age right now with ubiquitous CI/CD, collaborative tooling like Github, and more test frameworks than you can possibly name.

This velocity becomes a double-edged sword.  While it allows better, high-quality software to get to production quickly, that speedup often is met with an increase in feature additions. There's a key gap in the engineering process where communication and respect is absolutely crucial, and it's between the people who ask for and sell the features and the people who build out and test the features.

If this gap is not managed responsibly - and it is up to both sides equally to manage it - the gains in velocity will be lost by feature creep and overload.  Features will be added for a variety of reasons with varying degrees of attention toward the cohesion of the codebase.

## Security

Security is hard.  Not only are you in the nitty-gritty of systems programming, stopping stacks from being smashed without slowing your machine to a crawl - but some entity out there is actively picking apart those defenses.  Security is one of the few fields that is adversarial in nature, and it's not just 400-pound dudes in their mother's basement coming after me.  Malware developers work a 9-5 job just like you and me - many analysts tracking APT activity even report patterns like stopping to have lunch or taking 3 day weekends.

Security software is harder still.  You have all the overhead of caring for and feeding a live software project while highly paid and motivated hackers are having scrum meetings about how to best subvert every new feature you write.  On top of all this, _typical software constraints force you to be bad at your job_.  

## Conway's Law

One example of this is Conway's law.  Conway's law states that:

```text
Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure - Melvin Conway
```

What this means is that your company's reporting structure actually dictates the design of your system much more than any other factor.  If you have a 4 person team on a compiler, that compiler will have 4 passes.  It doesn't matter if it's optimal or not, it's human nature and I've very rarely seen a project that doesn't respect it.

Conway's Law is an issue because it creates a massive bias in the design of a system.  It's simple human nature.  Sure, you're going to get a working product - but is it the best it could be?  Odds are unless your org structure is set by a very thoughtful and technical manager, it's not optimal long term.

## Customer Requirements

When I worked in retail during college, I discovered something - "The customer is always right" is a MASSIVE lie.  Now deep into the software industry, I've found that remains true.  

## Less == More

## Security is Reactive

