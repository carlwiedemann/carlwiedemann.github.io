# 25 Learnings From 25 Years in Software

2026-11-22

In the fall of 2001, I started college. I didn't study computer science. What brought me to programming was that the university provided 10MB of Apache-hosted disk through a public subdomain with cgi-bin and FTP access.

For <strike>late adolescent vanity</strike> fun I made my own website with Microsoft Notepad, a pirated version of Adobe Photoshop 6, and a lot of copy-paste from W3C tutorials. Little did I realize those moments of simply following my curiosity would begin turning the flywheel of what became my entire career.

A quarter-century later, here are some things that I picked-up along the way, mostly through trial-and-error.

## 1. Code is for humans

What is code? It's just a bunch of text files. A densely layered onion of abstraction lies between those text files and what a machine ultimately does.

The point of abstraction is that we, the authors of the text, are *not* the machine. We are humans who use abstraction to try to instruct machines in more sophisticated ways than we could without abstraction.

It's fun to look at Quake III's [fast inverse square root](https://en.wikipedia.org/wiki/Fast_inverse_square_root) because of its cleverness. But its novelty also relies on its *lack* of abstraction. You wouldn't want an entire codebase to look like this, it would exhaust you rather quickly.

We spend more time reading and editing code we did not write instead of code we wrote ourselves. Legibility and clarity of code is what matters in building a great codebase, not cleverness. The moment you find yourself valuing your own work *because* it requires deeper insight to understand its value, you likely have been seduced by cleverness --- in spite of your collaborators who must read what you've written.

Write code for the person who isn't you. That person must clearly & quickly understand what you've done. That person doesn't live in your head.

## 2. Ergonomics matter

Computers are amazing tools. It is worth your time to continuously improve your use of the computer. When you notice repetitive toil, that signals a place to optimize. Time reveals new places.

Write many scripts for yourself. Keep your dotfiles versioned. Keyboard shortcuts are incredibly undervalued by most people ([Tampermonkey](https://www.tampermonkey.net/) and [Hammerspoon](https://www.hammerspoon.org/v1/) are my favorite utilities) --- and for your left pinky's sake, please [map caps lock to ctrl](/img/thank-me-later.png).

The less you have to think about what a tool is, the more you can think about what a tool does.

## 3. Rename it now

Code is written and time passes. Now something you named a year ago now seems like it was named wrong. "Naming things is hard". We all knowingly nod.

What makes it hard? Part of the difficulty is thinking of a *Good* name. But wait: You thought you had a *Good* name, now it turns out it was a *Bad* name. Now what if your new name you thought of --- which appears *Good* now --- *also* turns out to be *Bad*? You'd have to rename it again. So perhaps part of the difficulty of naming things is actually the difficulty of *renaming* things. Maybe if renaming things was easier, we might think naming things was easier because there would be less pressure to find a *Good* name in the first place.

Renaming things is hard for lots of reasons. But renaming a thing only gets harder the longer you wait to rename a thing. Effort aside, the cognitive load of a poorly-named thing sticking around slows down newcomers. "Oh yeah, we need to rename that". I hate saying this to people, don't you? Let's make it better for newcomers by being in the habit of renaming things we know need to be renamed. As best you can, the moment you notice a thing needs to be renamed, rename the thing.

(Idea: the first week of every quarter is *"rename week"*. Everyone gets to go do the renaming they know they should do.)

## 4. Label your arrows

Most of the time when I am sent a typical "boxes & arrows" diagram aiming to describe software the boxes are labeled but the arrows are not. This is a problem. (Yes, even "UML".)

What do unlabeled arrows mean? Only the author has the benefit of knowing for certain, but readers resort to something more vague: "Talks to", "Owns", "Knows about", "Observes", "Data flow". (Unfortunately, I suspect sometimes arrows simply mean which box was drawn first.)

It's easy to become overly focused on nouns rather than verbs. But it's the verbs that actually matter because data *moves* in systems. That's the whole point.

The real benefit comes when you find a label that doesn't make much sense --- you might have the wrong verb, or a missing noun. This friction inspires conversation that wouldn't have happened otherwise.

<figure>
<img alt="A pile of old television remote controls" src="/img/remotes.jpg" />
</figure>

## 5. Respect the user

Once I was building a feature that allowed users to customize a menu's options. The designer suggested mandating a user not configure an empty menu, so it would display a warning when users attempt removing the final option.

"It doesn't make sense to have an empty menu". Ok, sure. But what is the harm in allowing it? If we allow it, users just have an empty menu. We could measure how many users elect to have an empty menu (because maybe the menu wasn't useful to begin with). And we respect user choice, even if that choice "doesn't make sense". (This is true respect.)

Actively preventing this is adversarial and paternal for a consequence that plausibly creates the same outcome --- the *passive* friction of an empty menu might encourage using it, whereas the *active* friction of an error message forces them to.

Consider what you guard against. Does it matter? What might matter more is that your software doesn't coerce its users, even if the users want odd things. You have the opportunity to learn from them, by giving them space to act.

## 6. Say why, not what

Code comments are widely critiqued as redundant & dishonest. "Self-documenting code" apparently solves this.

This critique certainly rings true for much of the comments we see. But it is also circular reasoning because it simply describes *useless* comments. "The Bad thing is Bad". Ok, sure.

Can comments be useful? I believe so. People struggle to define what "useful" means. Ironically, the struggle itself is part of what makes comments useful because they *should* require a high price of admission to your codebase.

Here's my definition: Useful code comments provide *non-redundant context*. Don't describe *what* code does ([code already tells you that](https://www.commitstrip.com/en/2016/08/25/a-very-comprehensive-and-precise-spec/)), instead describe *why* code is the way it is: Why choice was made, future `TODO`s, known limitations & trade-offs.

Used sparingly in this way, truly useful comments don't get stale because they were never clutter to begin with. They illustrate the historical motivations the code itself simply takes for granted.

(In the agentic age, good comments are more important, because we are less reliant on what invisibly lives in our heads and more reliant on what lives in text.)

## 7. Consistently average beats sporadically great

Most of our time writing software is modifying existing code, not writing code from scratch. Some improvements are large, so we usually approach it incrementally. This is practical and reasonable, but often what we lack true commitment to *seeing it through*.

How often do you see different implementations of the same thing? When you ask your peers about it, you get some variation of the following:

* "Oh, yeah we never finished that migration"
* "Oh, yeah that's still being refactored"

With the half-finished migration/refactor, we could say *on average* code quality has improved by virtue of the better implementation, but in practice having two implementations is actually worse: Half-done work creates cognitive load to remember the nuances of both. The load degrades *your experience* in the code, and it is your experience in the code that matters, because the code *relies on it* to improve at all.

It is better not to *start* the change until you're prepared to *finish* the change.

## 8. Where do you want to manage complexity?

We cannot escape complexity in software. Fred Brooks [wrote best on the subject](https://www.cs.unc.edu/techreports/86-020.pdf).

Unfortunately, most people haven't read Brooks, and believe there's an oasis beyond the horizon. "If only we could just get this one thing right, then we would be fine! There will be a day when all our problems are gone!"

Solving one problem never leads to an unproblematic state, it (ideally) leads to more interesting problems to solve. But there will never be perfection. You have to decide *where* you want to try to isolate complexity, not *if*.

You may be frustrated with this repetition. Jerry Colonna calls it "[painting the bridge](https://www.reboot.io/wp-content/uploads/2021/12/Ops-Man-2-Edited-Transcript.docx.pdf)". (It takes so long to paint the bridge that once you finish painting the bridge you have to go back to the very start and begin painting the bridge again.) But that's the point. That's the point of all of this. That's actually where leadership and craftsmanship should aim.

Embrace the inevitability of complexity. Understand it as something that you cannot escape, but that you may shepherd to create lightness and brightness.

(Ironically, you may not want that world without problems in software because you would have very little to do. The fact that you have a job at all is testament to the fact that things need work to be done. There will always be work to be done.)

## 9. Visit the shadows

Greenfield is actually a myth in practice. Everything becomes Brownfield. Everything eventually becomes an artifact. It's easy to be happy with freshly-written code and a clean implementation. Most software isn't like that, even on the most prominent software used by hundreds of millions of users.

The unfortunate part of formalized education is it has too much Greenfield. You're not learning in legacy systems, but most of your time in life as an engineer will be working in legacy systems. Software is usually successful through making trade-offs that it then must endure. There are many shadows.

The courage to visit those shadows is a skill to embrace, not avoid. The most valuable and impactful times of my career are those when I have refused to stay comfortable and instead go looking for things in the dark places. This is where there's hidden value to be mined --- to discover changes we never knew we needed --- simply *because* they are unattractive.

Go exploring. Understand the checkered history. Become the engineer who is not so enamored by [the shiny things](https://blog.codinghorror.com/the-magpie-developer/), but the shadowy things.

<figure>
<img alt="'Amour' written on a Paris bridge, Notre Dame in the background" src="/img/paris.jpg" />
</figure>

## 10. It's not supposed to be easy

Engineers complain about naïve product managers and product managers complain about grouchy engineers.

Embedded in both complaints is a belief things could be different. But we should appreciate that generative work involves energy expenditure from opposing, reactant views.

A great way to appreciate non-engineers is *because* they are different from you. It is actually the difference that creates the product. Difference will bring difficulty, but through difficulty comes the reward. We need each other.

## 11. Delivery is design

The biggest mistakes made in software happen when people spend too much time building in isolation, without actually using or integrating into the adjacent systems. One rather nefarious form of isolation is isolation from the delivery mechanisms of what brings the software into becoming "live". Testing, delivery, & observation are accessorizing afterthoughts rather than pillars of the entire edifice.

Unfortunately, this is very typical, and I believe related to why <strike>bugs</strike> mistakes are typical. Nothing is real until it lives in the field, and it is the only real mechanism of feedback.

A better approach is aiming at delivery working & visible as soon as you possibly can: CI, tests, deployment, ingress/egress, basic observability --- even if yours is but a single dummy "hello world" endpoint, this isn't too soon. It doesn't have to be mature, but traveling the entire journey early will bring insight, and it all matures together. The journey is a form of design.

## 12. Learn C, learn Scheme, learn data structures, learn algorithms

It's very easy to complain about whichever languages and tools that you have to use at your day job. But everything didn't just happen spontaneously. Decisions that seem arbitrary or silly came through paradigms established decades ago. These are worth knowing, for both your own sanity and your own maturity.

You can get pretty far without learning many fundamentals, and just figuring things out as you go. (I speak from direct experience.) You can land a six-figure job just fine. But at some point you *will* hit a wall: Some bug you can't fix, some pattern you can't quite get right.

Faced with your own limitations, when you take it upon yourself to dive deeper into formalized knowledge you will begin to see your gaps, and recognize problems you have created. Decisions you felt confident making were quite fragile in reality.

This can be uncomfortable, but it is how you grow. As someone who took a circuitous path along this road, I suggest to fellow autodidacts that spending time with some fundamentals will only help.

## 13. You will change your mind

Part of learning is developing confidence and part of developing confidence is seeing the things you do to be correct.

But you can paint yourself into a corner where justifying yourself to be correct (in the name of developing confidence) becomes overly important. You can waste a lot of time pushing for decision through social friction, for something that may actually change --- it's called software because it's soft.

Accept you actually can't be correct all the time. You won't overly invest in any particular implementation that may need change. Relieve yourself of the burden of needing to predict the future.

More importantly, you'll model some humility visible to your peers, and your vulnerability helps dispel impostor syndrome that can keep people quiet. Done at the right moments, saying "I'm not sure" or "Let's try it and see" can actually create a healthier environment for junior peers to invest in learning and exercise some transparent humility of their own. Maturity comes to us all through a balance of confidence and curiosity.

## 14. Know the value of knowing

I studied civil engineering in college. A requisite course was construction management, which was, uh, "taught" by a guy named [Hyman Brown](/img/my-guy.jpg) who spent the majority of the course sharing lore about managing construction in the 1980s.

The only story I recall went something like this: On a construction site the most expensive piece of equipment is a crane. Contractors usually rent cranes, and pay by a day rate whether the crane is being used or not. Separately (and invariably), contractors find problems in the construction drawings. A pipe is drawn intersecting an elevator shaft. What to do? The formal process involves sending the architect an RFI "Request For Information" asking to provide an updated drawing. This of course takes time, and during that time the work may need to pause and contractor has to pay for the crane, even if it isn't being used.

But what are the possible ways the pipe can be fixed? Well, it can move left, or it can move right. So just skip the RFI and move the pipe.

> *"So I said 'move the pipe to the left!' I just saved us $30,000!"<br>--- Hyman Brown (probably)*

When we see problems in software, we've seen the value of exploring those shadowy depths to learn why a problem happened. But we should ask ourselves: If we investigate and learn the root cause of the problem, would it actually change anything about how we fix the problem? Or are we just spending time learning without a practical outcome?

Sometimes the answer is important. Sometimes the answer isn't important. Sometimes, despite being important, you can't budget time to find the answer now, you have to fix the problem you know you have.

And sometimes even when you try to figure out the answer, you won't. Instead, the answer will arrive years later when you're walking your dog or making a sandwich or stepping out of the shower. There's a lot you won't know. Some of it should be because you sense the value of knowing.

<figure>
<img alt="Cranes on the Colorado horizon" src="/img/cranes.jpg" />
</figure>

## 15. Let it crash

Exceptions are part of software systems. Things will go wrong! Sometimes you can predict this, sometimes you can't.

When things aren't stable, the temptation is to make individual procedures more robust somehow. But what does that mean in practice? If you try to anticipate every single thing that will go wrong, you end up with something that is very complicated. When a problem happens, what should each downstream component do? Is there a sensible way to continue? There may not be. What to do then?

You may think that more sophisticated error handling is the way out of this. The problem is that error handling can also have errors. You may mistakenly handle the mistakes. Then when you try to fix the mistakes that are handling the mistakes, you may make other mistakes. Now you have three mistakes. Which mistake is the real mistake? This is a very bad place to be. You'll spend more time dealing with your own mess than anything useful.

This view is why things like Kubernetes have been successful. We know systems will have problems, so when they have problems let's have ways to respond. Let's care less about what the problem is. Let it crash.

"Let it crash" is quite different from viewing the world through the lens of more defensive idioms, e.g. `try/catch` or tuple/monad returns. (What works for low-level tooling doesn't really work for high-level tooling.) When you zoom-out to the bigger picture, you simply don't know when you don't know. Don't try to guess. Log, then exit.

## 16. Approach cannot be decoupled from scale

Two phrases I used to use that I now use less and less: "best practice" and "performant". These describe absolute states. But claims are always relative to something. 50µs < 50ms. Does that make it better? Why not 50ns? It's not what the "right" answer *is*, it's *why* you pick the answer for your situation.

I've seen engineers concern themselves more with scale that does not currently exist and being blind to the problems that do exist. Do you need the most sophisticated/decoupled/modular architecture to solve a problem? Are you even solving the problem you have? Maybe you aren't even starting on your problem because you have mentally over-engineered the solution. 

Does that mean that you've made the "best" choice? Maybe, maybe not. Worry less about this, just try to recognize when you have outgrown your implementation. Shed the skin, but only when it is time.

## 17. Be prepared to delete it

Programming is a craft. Part of being a craftsperson is knowing that the thing you made came from you. It exists as an artifact in the world. If someone else made it, it might look different.

Your ego will be very tempted to attach to what you make. That can be very motivating, but equally often, **"You"** gets in the way.

Zoom-out. What are you really trying to do? You're really trying to solve a problem. Sometimes solving the problem involves taking the thing you made and throwing it out. Maybe it was just a bad idea. Try something else.

The craftsperson who can separate their own sense of self from the output of their mind has true resilience and power, and can adapt to actually be useful. The purpose of the craft is to be useful --- thus follows the purpose of the craftsperson.

## 18. Step away

You are going to get stuck. What to do? The answer lies in the question, because it was *doing* that got you stuck. Why would you keep *doing*? It just got you stuck! Maybe you should stop doing. "But I'm stuck!" Yes. So *do* something different. Or just *don't do* for a while. Find a [hammock](https://www.youtube.com/watch?v=f84n5oFoZBc). The answer will come.

## 19. It might just take time

Story points. Planning poker. Fibonacci. Burndowns. "Pigs & Chickens". Velocity. All of these are just ways of trying to understand ourselves. Ultimately these processes aim for structured conversation.

I've been on many sides of this. I've worked for myself freelance just doing what I want. I've worked in scrum teams. I worked in Kanban teams. I've done strict pairing/mobbing XP, story mapping, inceptions. These days (quite oddly) it appears I am back to just doing what I want?

Here's what I think actually works:

* Estimate whether it's going to be done in **hours**, or **days**, or **weeks**, or **months**, or **years**
* Work on the most important thing, don't context switch
* Keep working on the thing until you are satisfied with its position (this may not mean "done")
* Move on to the next thing
* Repeat

Sometimes "satisfied" means we finish it the way we thought, other times "satisfied" means it's taking too long so we'll leave as-is & go do something else. We have to use our gut and our experience.

What about stakeholders? They want to be able to predict things. But a lot of times when we bring them an estimate, we have some version of the following conversation:

* You: "It's going to take 18 months"
* Stakeholder: "That's too long"
* You: "...ok, so how long do you *need* it to take?"

How long do you need it to take? How long do you want to budget in order to see something? You don't go shopping for a car or a house without a budget. So don't go shopping for features without a budget.

That's actually the first question we should ask: What's the budget? So just ask that question and then decide *what* you can reasonably build in *that* period of time. It may be crude. But then you can decide if it's worth the time to keep going. That's real agile.

This is difficult to do, and what makes it difficult is accepting it is inexact. But it's about dialogue, not precision.

<figure>
<img alt="Waiting beneath the shoefiti by the Prague Metronome, " src="/img/prague.jpg" />
</figure>

## 20. What's not being said?

I've worked with two equally irritating types of engineers: those who tend to say Yes, and those who tend to say No. The former are well-intentioned but lack vision; the latter are passionate but lack empathy. Teams of engineers can drift toward either of these behaviors (and others) as a default mode.

As an individual, a much better place to occupy is the place that isn't being filled. That isn't a static place. It depends on the dynamics of the group.

If everyone is saying Yes, try to find gaps. If everyone is being difficult, try to find common ground and build bridges. If everyone is too talkative, defer to action. If too quiet, ask rhetorical questions.

If you are working cross-functionally across multiple teams then you may need to play multiple of these roles at the same time.

I have made many mistakes trying to figure this out. Don't just be frictional or blow things out of proportion. This will degrade your social capital. People need to respect you if you are to make progress. Seek the things that nobody else sees.

When faced with a disagreement or a decision point, a great way to proceed is accept the terms of the opposing view then explore the outcomes. If you are skeptical of *X* then you may have an easier time granting *X* as a given, then exploring its outcomes rather than trying to refute *X* directly.

## 21. Tautologies are inevitable

Software teams organize through different rituals. Which one is right for you? This is actually a cultural issue. There's no sort of platonic ideal of software process that we are all aiming toward, despite the tales of successful processes.

"But Spotify!" The tales of successful processes always end with the tacit promise that if you switch to use that process you'll see the same success. Many of these things are simply self-referential and circular: Someone who happens to have success and believes whatever steps taken toward success are the steps that lead to success in general. But that success is local to them. It only works if the culture and minds forged come along for the ride. It's all a package.

If we're going to make a larger claim about what works for everybody, we're probably wrong. People like comfy universalities. Core values can be helpful guides when we are lacking empirical pointers, but sometimes when trying to understand ourselves, we simply point back at the value that is pointing at us (or perhaps beyond us).

## 22. Quit complaining about tech debt

Earlier in my career, I was angry when I saw tech debt. "Why did anyone allow this??" I resented people for being so careless to allow it in the codebase.

Ironically, I was seldom given an opportunity to address it --- I could simply complain. Given that opportunity to address it, I'd face the responsibility of *not* introducing new tech debt. To believe that *if it was all up to me*, I would do everything right was an extremely naïve and immature understanding of how software is actually built.

We are ultimately going to make mistakes --- we don't even realize mistakes we're making right now: decisions that we make are later revealed as incorrect. This is inevitable.

If you are grouchy because of mistakes, then you are divorced from reality. The grouchy engineer is a well-trod stereotype, and one you should avoid. You will alienate people. Of course, it is easy to be grouchy! Most code is bad! But that's why you're here.

Instead, remember you want to be useful. The engineer who complains without changing minds isn't useful. Being useful means actually contending with opaque constraints. You must make trade-offs --- and make trade-offs *between* trade-offs. Which fires do you extinguish? Which do you let burn? You *will not* be perfect. You *cannot* be perfect. Perfection is the lowest standard because it is impossible.

Tech debt is a tool just like financial debt is a tool. Your mortgage debt allows you to do something few people could otherwise. Tech debt is the same. It allows us to gain things. But as we must be reverent of financial debt, tech debt requires the same. So be open to using it, manage it, and discuss it openly. But: Use it. Don't complain about it.

## 23. Incentives will win, always

Whatever system we end up working on will have problems. But the cause of many problems is not pure technicality or budget constraint, but that fixing the problem is not incentivized by the working environment --- that anybody with authority does not value its fix over its harm.

The lens of pure incentive and interest drives things much more than any sort of good implementation or anything else. Many engineers have a mistaken belief that explaining problems in purely technical terms will be enough to change minds, simply because the explanation technically sound. When non-technical people don't agree, this can be a source of frustration and resentment. But what they are actually doing is responding to incentives. Their incentives are fundamentally misaligned with yours.

It can be a bitter pill to swallow, but as the craftsperson you are likely going to have to be employed by people who are not craftspeople, and proceed in a delicate alliance.

The craftsperson is usually assessed against something different than the quality of the craft, but rather how the craft comes to benefit the owners of the company. This will constrain the choices, and possibly discourage spending the requisite time to make the technical change that is *The Ideal* solution. Incentives aren't always aimed at *The Ideal*. Incentives are larger than code.

This can seem like a depressing world, but I think accepting the difference and divergence actually creates more empathy and less frustration. Software happens to be a vehicle for the company. You have been hired not because of what the software is but what the software does: To make one dollar turn into more dollars.

Very few places in the world exist for the purpose of simply making software for software's sake. Most of us are navigating the alliance between the craft as an *End* and the craft as a *Means*.

Zoom-out. Realize that as much as you value craft, it is also a means to provide you & your family. The enjoyment of craft *can* exist within your job, and I was fortunate enough to be able to enjoy this for a long time. But things can change. Sometimes a job is just a job. Your job can get pretty annoying and pretty frustrating. You may feel like you're always just putting lipstick on the pig or shaving the yak. But accept that by taking the job, you have agreed to live in the world of incentives that interest the owners of the company. (And hopefully this choice has been lucrative --- it is software, after all --- there are a lot worse things to do.)

Next time you are preoccupied with the annoyances and frustrations --- perhaps just as you've ended your workday to sit down at the dinner table: Stop. Look around. See the life you have the opportunity to experience. Your world is bigger than the craft.

## 24. Everything changes, nothing changes

What it means to be a software engineer in the last year has radically changed more so than I've ever seen in the last 25 years. I've gone from being someone typing out the code to someone who supervises robots trying to do the same thing, sometimes succeeding, sometimes failing --- sometimes both succeeding and failing at the same time.

If you love the craft, it is a very difficult time to be a software engineer. The fundamental thing that you enjoy doing in your day job has largely gone away, or at least changed drastically.

If you are at the start of your career, the skills you need to develop to actually assess and understand whether the robots are going in the right direction or not may be elusive. Can you learn from the robots? The problem is that the robots can sound pretty convincing and build something fairly plausible *from the outside*. That's a very different experience from being *on the inside* just getting damn stuck yourself. I must say there's something nice actually in retrospect about getting damn stuck because it forced you to really think through things in a way that you haven't had to before.

I'm concerned that the real force of necessity which drives personal growth becomes diminished for generations to come.

Despite all these things so many things actually don't change at all. Many of the fears people have about the nature of the work fundamentally disappearing are completely overblown.

* *"We won't need junior engineers anymore because AI agents are like junior engineers!"*
* *"We won't need senior engineers anymore because now junior engineers can be like senior engineers since they have AI agents!"*

Neither of these are playing-out. Instead, I predict everything just kind of "shifts right" on the output curve. Juniors become a bit more productive, seniors become likely even more productive.

It's kind of like giving everybody electricity. Having electricity meant that we could create power tools, and it was a lot easier to screw together the framing for a house. But the invention of the power tool did not result in a glut of unemployed architects who designed houses. And despite having electricity, we still have poverty and strife in the world. We just have different tools.

And true builders still need particular tools. If you have used power tools, you know that sometimes you actually do need the screwdriver. You actually do need precision in a way the power tool does not afford you.

Coding was never really your job. Your job is to be a critical thinker. It's actually the thinking, manifested in the choices and forces that exist within the artifact. That is what your craft is, robots or not. Code shaped by someone thinking critically looks very different than when not.

All that said, there is tedium in building software, which I am happy to hand that off to the LLM. I don't really want to fuss through why removing a certain header file now the damn thing won't compile anymore because of some sort of implicit dependency that people have missed for years and years. That is pure tedium. Let the LLM do that. Let me think about something else. Thank you, LLM!

I don't think this world is going away. Just as when the search engine arrived, these robots are the new search engines. You can work without a search engine, but you will limit your experience.

What happens 25 years from now? I hope to share more then.

<figure>
<img alt="Flowers pushing through the pavement on your street" src="/img/flowers.jpg" />
</figure>

## 25. Relax

You will never stop having impostor syndrome. I've been doing this for over two decades and I still feel it.

But I have good news: Real impostors don't get impostor syndrome. Real impostors *know* they are impostors, rather than *fear* they are impostors.

So relax, because if you have impostor syndrome, it means you aren't an impostor.
