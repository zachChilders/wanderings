

Defense in depth means very different things to very different people.  "Add as much as you can with the money you can spend" is traditional, but that has issues in modern development.  A thread.

A significant amount of security wisdom was developed like 30 years ago.  That doesn't make it bad or wrong, but it does mean we need to re-examine things.

The goal of Defense in Depth is to make sure you're not low hanging fruit. With every layer of defense you add, you cost X more effort/skill to hack and attackers go elsewhere.

This TOTALLY makes sense when you're working with raw infrastructure.  WAFs, Firewalls, SELinux, and k8s policies are either a one time cost or amortized over large fleets. 

Meanwhile feature devs are making sure JWTs are secure, implementing server side logic checks, pinning certificates, using memory safe languages and practices - that sort of thing.

The trouble is that in a lot of orgs, Defense in Depth implementations manifest themselves as checklists, and those checklists are probably written by someone who isn't deploying every day.

Giving a lot of benefit of the doubt here - but most engineers aren't knowingly negligent when it comes to security.  We're paid good money and on the whole want to Do the Right Thing™️

The checklist folks mean well too!  Everyone wants this to work, but security can't be one size fits all.

Checklists work okay for infra teams.  You can translate items into code requirements with Docker, Ansible, etc. Infra is long(er) lived and if it's all running smooth, you can iterate on it overtime.

Most production codebases are optimized to get changes out quickly and without friction though - the exact opposite of running through a checklist!

Imagine though if a codebase like chromium had a whole security review on every checkin - they'd go from hundreds of PRs a day to a small handful, and pay through the nose for it!

In the dev world, security checks need to be triaged ahead of time and encoded as meaningful (but cheap) security tests on branch policies.

That's not to say security reviews are outdated - just they take much longer than a growing business can keep up with.  

A well oiled codebase with enough engineers on it won't even look the same by the time a full review completes.

The REAL bang for the buck in the dev world is protecting the codebase itself - making sure that only the people authorized to commit can and can do so often.

This approach, coupled with regular security reviews of sensitive areas and the deltas, is MUCH more sustainable to the code folk.