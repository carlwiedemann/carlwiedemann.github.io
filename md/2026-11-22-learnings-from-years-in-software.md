# 25 Learnings From 25 Years in Software

2026-11-22

In the fall of 2001, I went to college. I didn't study computer science, but all students received 10MB of university-hosted disk, served through Apache on a public subdomain, with cgi-bin and FTP access. For <strike>late adolescent vanity</strike> fun I made my own website with Microsoft Notepad, a pirated version of Adobe Photoshop 6, and a lot of copy-paste from W3C tutorials. Little did I realize those moments of simply following my curiosity would begin turning the flywheel of what became my entire career.

A quarter century later, here are some things that I picked-up along the way, acquired mostly through trial-and-error.

## 1. Code is for humans

What is code? It's text files that describe instructions for a machine. A massively layered onion of abstraction lies between what those text files contain and what the machine ultimately does.

The point of abstraction is that we are *not* the machine. We are humans who use abstraction to try to instruct machines in more sophisticated ways than we could without abstraction.

It's fun to look at Greg Walsh's [fast inverse square root](https://en.wikipedia.org/wiki/Fast_inverse_square_root) because of its cleverness. But its novelty also relies on its *lack* of abstraction. Puzzling through it is novel, but you wouldn't want an entire codebase to look like this code, it would exhaust you rather quickly.

Most of us will spend more time reading and editing code we did not write than code we wrote ourselves. Even in the "agentic" age, legibility of code is what matters in building a great codebase, not cleverness. The moment you find yourself thinking you've written something really good *because* it requires deeper insight to understand, you have likely been seduced by cleverness, in spite of the people who also have to read what you've written.

So write for the person who isn't you, but must clearly & quickly understand what you did. That person doesn't live in your head.

## 2. Ergonomics matter

The ease with which you use the computer will improve your output with the computer. What's important is to *keep* spending time improving how you use it. When you notice repetitive toil, that should signal a place to optimize. Time will reveal new places.

Write lots of little scripts for yourself, keep your dotfiles in version control. Keyboard shortcuts are incredibly undervalued by most people (Tampermonkey and Hammerspoon are my favorite utilities) --- and for your left pinky's sake, please map caps lock to ctrl.

The less you have to think about what the tool is, the more you can think about what the tool does.

## 3. Rename it now

Code is written. Time passes. Things change. Something you named a year ago now smells like it wasn't named correctly. "Naming things is hard", we all knowingly nod.

And what makes it hard? Part of the difficulty is thinking of a good name. But wait: You thought you had a good name, right? So your new good name also may also be bad. So it appears that a separate part of the difficulty is the *toil of renaming things*. If renaming things was easier, we might think naming things was easier because there would be less pressure to get it right.

Renaming things is hard for lots of reasons. But renaming a thing only gets harder the longer you wait to rename a thing.

So as soon as you notice a thing needs to be renamed, rename it. You likely won't regret doing so because renaming will only become more difficult in the future. And the cognitive load of a poorly-named thing slows down newcomers. "Oh yeah, we need to rename X." I hate saying this to people, don't you? Let's make it nice for the newcomers by renaming the things that we know need to be renamed.

(I have an idea that the first week of every quarter should be "rename week" where every team gets to go do the renaming they know they should do.)

## 4. Label your arrows

Most of the time whenever I see a "boxes & arrows" diagram trying to describe a software system (even if it is claimed to be UML) the boxes are labeled but the arrows are not. This is a problem.

What do the unlabled arrows mean? Nobody can know for certain, they'll assume something vague like "Talks to", "Owns", "Knows about", "Observes", "Data flow". (Unfortunately, I believe sometimes arrows simply mean which box was drawn first.)

I suspect people forget to label arrows because they are overly focused on nouns rather than verbs. But it's the verbs that actually matter because data *moves* in systems. That's the whole point.

The real benefit of labeling arrows is when you find a label that doesn't make much sense. You might have the wrong verb, or the verb actually reveals a missing noun. This friction inspires conversation that wouldn't have occurred otherwise.

<figure>
<img alt="Remote conrols" src="./img/remotes.jpg" style="max-width: 30rem; width:100%;"/>
</figure>

## 5. Respect the user

Once I was building a feature that allowed users to customize a menu. Menu options could be added or removed. My product manager suggested we mandate a user must have at least 1 option exist in the menu, and subsequently display a warning modal if a user tries removing the single option, as doing so would result in an empty menu.

"It doesn't make sense to have an empty menu," says the PM. Ok, sure. But why prevent a user from doing this? What is the harm in allowing it? If we allow it, then the user will just have an empty menu and will probably then realize that maybe putting something in the menu would be better instead. We also get the insight of collecting metrics on how many users elect to have an empty menu --- maybe the menu wasn't useful to begin with --- we can know that if we allow & track empty states. And we are at least respecting user choice, even if that choice "doesn't make sense." This is true respect.

If instead we show the warning, we are just being adversarial and paternal for a consequence that actually likely informs the same outcome for the user; the passive discomfort of an empty menu vs the active discomfort of an error modal.

Consider what you guard against. Does it matter? What might matter more is that your software doesn't coerce its users, even if the users want odd things. You have the opportunity to learn from them by giving them space to act.

## 6. Say why, not what

Code comments are widely critiqued. People say they are redundant & lie about code while truly "self-documenting code" should be preferred.

This critique certainly rings true for much of the code we see. But it is also circular reasoning because it simply describes *useless* comments. "The Bad thing is Bad." Ok, sure.

Can comments be useful? I believe so. People struggle to define what useful comments are. Ironically, the struggle itself is part of what makes comments useful because they should require a high price of admission to your codebase.

Here's my definition: Useful code comments provide non-redundant context. Don't describe *what* code does (because code already tells you that), instead describe *why* things are the way they are: Why choice was made, future `TODO`s, known limitations & trade-offs. They tell the story of the coding process, not the code itself, which tells its own story.

Used sparingly in this way, truly useful comments are less likely to get stale because they were never clutter to begin with. They illustrate the historical knowledge that the code itself simply takes for granted.

(In the agentic age, this can really only help, because we are less reliant on what lives in our heads and more reliant on what lives in text.)

## 7. Consistently average beats sporadically great

Most of our time writing software is trying to improve things that have already been written, not writing something from scratch. When we improve things, it can be a large effort, so we approach it incrementally. This is practical and reasonable, but what is usually missing most of the time is the true commitment to *see it through*.

How many times have you been looking at code and you see two different implementations of the same thing? And when you ask about it, someone says:

* "Oh yeah we need to finish that migration"
* "Oh that's being refactored"
* "Oh yeah that isn't quite done yet"

I hate saying this to people. Don't you?

Even though you could say on *average* the overall quality has "improved" by adding new code in the second, better system, having two systems is actually a worse situation. We should be more vigilant about *avoiding this situation* because half-done work creates cognitive load. What has not improved is *your experience in the system* and ultimately is your experience in the system that matters, for it is what the system relies on to improve at all.

It is better to not change anything at all until you're prepared to invest in changing all of it rather than to create two systems that exist in perpetuity.

## 8. Where do you want to manage complexity?

We cannot escape complexity in software. Fred Brooks [wrote best on the subject](https://www.cs.unc.edu/techreports/86-020.pdf).

Unfortunately, most people haven't read Brooks. "If only we could just get this one thing right, then we would be fine! There will be a day when all our problems are gone!"

(Ironically, you may not want that world because you would have very little to do. The fact that you have a job at all is testament to the fact that things need work to be done. There will always be work to be done.)

Solving one problem never leads to an unproblematic state, it (ideally) leads to more interesting problems to solve. But there will never be perfection. You have to decide *where* you want to have problems, not *if*. Which layer?

I use "interesting" in the sense of "may you live in interesting times." Interesting is not always better. Understand that when you solve one problem, another one will open up.

You may feel frustrated with this. Jerry Colonna calls this "[painting the bridge](https://www.reboot.io/wp-content/uploads/2021/12/Ops-Man-2-Edited-Transcript.docx.pdf)." It takes so long to paint the bridge that once you finish painting the bridge you have to go back to the very start and begin painting the bridge again. But that's the point. That's the point of all of this. That's actually what leadership and craftsmanship is.

Embrace the inevitability of complexity. Understand it as something that you cannot escape, but that you may shepherd in ways that make things a bit lighter and brighter.

## 9. Visit the shadows

Greenfield is a myth. Everything becomes Brownfield. Everything becomes an artifact. It's easy to be happy with freshly-written code and building a nice, clean implementation, but that is why maintaining software is difficult, because most of the time what the work requires is not easy in this way.

The unfortunate part of formal education is that it only really teaches people the Greenfield work. You're not learning in legacy systems, but most of your time in life as an engineer will be in legacy systems. That's just the nature of work. Something has been successful. It's successful because it had to make trade-offs, and we will face the impact of those decisions for years to come.

This is a skill to embrace, not avoid. The most valuable and impactful times of my career are those when I have refused to stay in the nice newly written area and instead visit the nasty gross legacy code.

Become the engineer who is not so enamored with shiny things, become the engineer who is more enamored by shadowy things.

<figure>
<img alt="Paris" src="./img/paris.jpg" style="max-width: 30rem; width:100%;"/>
</figure>

## 10. It's not supposed to be easy

How many times have you heard developers complain about product managers? How many times have you heard product managers complain about developers?

Embedded in the complaints is the belief that things could be different. But something worth appreciation is that the generative nature of work involves the expenditure of energy from opposing views. Reactants encounter each other, and product is created through the exchange of the energy embedded within.

The best way to work with people is to appreciate that they are different from you *because* they are different from you. It is the difference that creates the product. The difference will bring difficulty. Through the difficulty comes the reward.

Arrive with reverence for the difficulty, not disdain.

## 11. Delivery deserves design

The biggest mistakes made in software happen when people spend too much time building in isolation, without actually using or integrating what they've built into the adjacent systems. Moreover, a more nefarious form of isolation is the isolation from the delivery mechanisms of what brings the software from a developer workstation into a live environment. These are seen as accessorizing afterthoughts rather than pillars of the entire edifice.

Unfortunately, this is very typical, and I believe this is why <strike>bugs</strike> mistakes are typical. Even people who believe they are getting "fast feedback" may only be getting feedback isolated to a particular unit. Nothing is real until it lives in the field, and that will be the real mechanism of feedback.

A way to achieve this is to get the entire ecosystem from development to deployment working and visible as soon as possible: CI, tests, deployment, ingress/egress, observability. Even if you have a single dummy endpoint that is just "hello world", if it can be fully integrated this It doesn't have to be mature, but the entire journey should be traveled. Use the journey as a form of design. "Design" is a verb.

## 12. Learn C, learn Scheme, learn data structures, learn algorithms

It's very easy complaining about the programming languages and tools that you have to use at your job. But everything you're doing and seeing on a computer didn't just happen accidentally. It can seem like a lot of decisions are arbitrary or silly, but paradigms established decades ago led to everything we see today, backward or confusing as they will appear.

You can get pretty far without learning anything of the fundamentals and just figuring things out as you go (speaking from experience). That can get you a six-figure job just fine. But at some point you will hit a wall, and upon delving deeper into formalized knowledge you will see a lot of gaps in your own work. You will see the decisions you felt so confidently making were quite fragile.

This is a normal part of learning, but I suggest to everyone that spending time with some fundamentals will only help the autodidact. I am still catching-up.

## 13. You will change your mind

Part of learning is developing confidence and part of developing confidence is seeing the things you do to be correct.

But you can paint yourself in a corner where being correct becomes actually really important to you. A form of bike-shedding is spending too much time discussing something that is actually truly ephemeral. Software is soft.

Accepting you actually can't predict the future will benefit you in several ways. Firstly, you won't overly invest in any particular implementation. Secondly, and more important benefit, is that you'll exercise some humility that is hopefully observable to your peers. This vulnerability helps dispel the impostor syndrome that surrounds everyone.

Done at the right moments, saying "I'm not sure" or "I don't know" can actually create a healthier environment for junior peers to invest in change, seek experimentation, accept uncertainty, and exercise humility of their own. Maturity comes through a balance of confidence and curiosity.

## 14. Know the value of knowing

In college I studied civil engineering. A requisite course was construction management, which was, uh, "taught" by a guy named Hyman Brown who spent the majority of the course sharing lore about managing construction in the 1980s.

The only story I recall went something like this: On a construction site the most expensive piece of equipment is a crane. Contractors usually rent cranes, and pay by a day rate whether the crane is being used or not. Separately, and invariably, on construction projects, contractors find problems in the drawings. A pipe is drawn intersecting an elevator shaft. What to do? The formal process for dealing with this is sending the architect an RFI "request for information" asking to provide an updated set of drawings. This, of course, takes time, and during that time the contractor has to pay for the crane, even if it isn't being used.

What are the possible ways which the pipe can be solved? Well, it can move left, or can move right. So just skip the RFI and move the pipe.

> *"So I said ‘move the pipe to the left!'…I just saved the project thirty thousand dollars!!" --- Hyman Brown*

When we see problems in software, often we're inclined to dive into the rabbit holes (being the fixers that we are), but we should ask ourselves: If we actually learned the root cause of the problem, would it change anything about how we fix the problem? Or are we just spending time we don't need to spend?

Sometimes the answer is important. Sometimes it's not important. Sometimes, despite being important, you can't budget time to answer it now, you have to wait. And sometimes even when you try to figure it out, you won't. Instead, the answer will arrive years later when you're walking your dog or making a sandwich or stepping out of the shower. There's a lot you won't know, and some of it should be because you sense the value of knowing.

<figure>
<img alt="Prague" src="./img/cranes.jpg" style="max-width: 30rem; width:100%;"/>
</figure>

## 15. Let it crash

Exceptions are part of software systems. Things will go wrong! Sometimes you can predict this, sometimes you can't.

So you want your system to be stable and when things aren't, the temptation is to make the system more robust somehow. What is making the system robust? If you try to anticipate every single thing that will go wrong, you end up with something that is very complicated.

Ultimately in call stacks there's earlier parts and later parts. When a problem happens, what should each later part do? Is there a sensible way to continue? There may not be. What to do then?

You may think that more sophisticated error handling is the way out of this. The problem is that error handling can also have errors. You may mistakenly handle the mistakes. Then when you try to fix the mistakes that are handling the mistakes you may make other mistakes. Now you have three mistakes. Which mistake is the real mistake? This is a very bad place to be. You'll spend more time dealing with the ways you dealt with mistakes than just moving on.

This view is why things like Kubernetes have been successful. "We know systems will have problems, so when they have problems let's have ways to respond." Let's care less about what the problem is.

"Let it crash" is quite different from viewing the entire world through the lens of `try/catch` or tuple/monad returns. What works for low-level tooling doesn't really work for high-level tooling. When you zoom-out to the bigger picture, you simply don't know when you don't know. Don't try to guess. Exit. Log what's going on. Be upfront about what's happening.

As a user of software, I would rather interact with the system that is honest & flaky than the system that is dishonest. Be honest about what you know. Sometimes that's just letting the bloody thing crash and logging what you can.

## 16. Approach cannot be decoupled from scale

Two phrases that I used in the past that I don't really use anymore: "best practice" and "performant". These describe absolute states. But claims are always relative to something. 50 µs is faster than 50 ms. Does that make it better? Why not 50ns? It's not what the answer is, it's why you pick these things.

I've seen engineers concern themselves more with scale that does not currently exist and being blind to the problems that do exist. Do you need the most sophisticated decoupled perfect architecture to solve a problem? That can be so daunting you might not start. Does that mean that you've made the right choice for now? Maybe, maybe not. Worry less about this, but recognize when you need to change.

It's not about being right now, it's about recognizing when you have outgrown your implementation. Shed the skin. Try something else.

## 17. Be prepared to delete it

Programming is a craft. Part of being a craftsperson is knowing that the thing you made came from you. It exists as an artifact in the world. If someone else made it, it might look differently.

Your ego will be very tempted to attach to what you make. That can be very motivating, but equally often, you get in the way.

Zoom out. What are you really trying to do? You're really trying to solve a problem. Sometimes solving the problem involves taking the thing you made and throw it in the rubbish bin. Maybe it was just a bad idea. Try something else.

The person who can separate their own sense of self from the things that come out of their mind has true resilience and power, and can adapt to actually be useful. The purpose of the craft is to be useful, so follows the purpose of the craftsperson.

## 18. Step away

You are going to get stuck. What to do? The answer lies in the question, because it was *doing* that got you stuck. Why would you keep *doing*? It just got you stuck. Maybe you should stop doing. "But I'm stuck!" Yes. So *do* something different. Or *don't do*. Find a [hammock](https://www.youtube.com/watch?v=f84n5oFoZBc). The answer will come.

## 19. It might just take time

Story points. Planning poker. Fibbonachi. Burndowns. "Pigs & Chickens". Velocity. All of these are just ways of trying to understand ourselves. Ultimately these processes exist to have some structure to our conversations.

I've been on many sides of this. I've worked for myself freelance just doing what I want. I've worked in scrum teams. I worked in Kanban teams. I've done strict pair programming XP, story mapping, inceptions. These days (quite oddly) I am back to just doing what I want?

Here's what I think actually works:

* Estimate whether it's going to be done in hours, or days, or weeks, or months, or years
* Work on the most important thing, don't context switch
* Keep working on it until you are satisfied with its position (this may not mean "done")
* Move on to the next thing
* Repeat

Sometimes "satisfied" means we finish it the way you thought you would, but other times "satisfied" means it's taking too long and we're just gonna cut  losses or drop it & to go do something else. We have to use our gut and our experience.

What about stakeholders? They want to be able to predict things. But a lot of times when we bring them an estimate, we have this conversation

* Me: it's gonna take 18 months.
* Stakeholder: That's too long.
* Me: …Ok how long do you need it to take?

How long do you need it to take? How long do you want to budget in order to see something? You don't go shopping for a car or a house without a budget. So don't go shopping for features without a budget.

That's actually the first question we should ask: What's the budget? So just ask that question and then decide what you can reasonably build in *that* period of time. It may be a totally ham-handed version of what it will eventually become. But then you can decide if it's worth the time to keep going. That's real agile.

This is hard to do, and the hardest part about it is accepting it is inexact.

<figure>
<img alt="Prague" src="./img/prague.jpg" style="max-width: 30rem; width:100%;"/>
</figure>

## 20. What's not being said?

I've worked with two equally irritating types of engineers: those who tend to say yes, and those who tend to say no. The former are well-intentioned but lack vision, the latter are passionate but lack empathy. Teams of engineers can drift towards these behaviors (and others) as a default mode.

A much better place to occupy is the place that isn't being filled. That isn't a static place. It depends on the dynamics of the group.

If everyone is saying yes, then I try to find gaps. If everyone is being difficult, I try to find common ground and build bridges. If everyone is too talkative, I defer to action. If too quiet, I prompt rhetorical questions. If working cross-functionally across multiple teams then I may need to play multiple of these roles at the same time.

I have made mistakes. We shouldn't just be a gadfly, people need to respect us if we are to make progress.

For technical conversation, we should seek gaps that nobody else sees. Don't blow things out of proportion, that will just degrade our social capital. But a great way to do this is to walk down the road a bit. If we are skeptical of *X* then we may have an easier time by granting it as a given condition and then exploring holes in  downstream outcomes of *X* rather than trying to refute *X* directly.

## 21. Tautologies are inevitable

Software teams organize through different rituals. Which one is right for you? This is actually a cultural issue. There's no sort of platonic ideal of software process that we are all kind of aiming toward.

"But Spotify!" Stories of successful processes always end with the tacit promise that if you switch to use that process you'll see the same success. Many of these things are simply self-referential and circular: You happen to have success and so you believe whatever steps you took toward success are the steps that lead to success in general. But your success is local to you.

If you're going to make a larger claim about what works for everybody, you're probably wrong. People like comfy universalities. At my current job, the culture is basically this: Hire ambitious people, hold them to a high standard, let them organize however they want.

## 22. Quit complaining about tech debt

Earlier in my career, I would be unequivocally peeved when I saw tech debt. "Why did anyone allow this??" I resented people for being so foolish and careless to introduce it to the codebase.

Ironically, the luxury I enjoyed was seldom being granted the opportunity to fix it --- I could simply complain. Given the opportunity to fix it, I would face the responsibility of truly paying it down without introducing new tech debt. To believe that --- *if it was all up to me* --- I would do everything right was an extremely naïve and immature understanding of how software is actually built.

We are ultimately going to make mistakes. We don't even realize the mistakes we're making right now: There will be decisions that we'll feel on the fence about and ultimately have to puzzle through, and we'll make a decision, and feel good about it. But in hindsight, we realize that that decision was wrong. This is inevitable. This is just going to happen.

The grouchy developer is a well-trod stereotype, and one you should avoid. It is easy to be grouchy! Most code is bad!

Instead remember you want to be useful. The person who always complains without changing anyone's mind isn't useful. Being useful means actually contending with this reality: The constraints surrounding what you're building are often opaque. You're going to have to make trade-offs --- and make trade-offs between trade-offs. Which fires do you extinguish? Which do you let burn? You will not be perfect. You cannot be perfect. Perfection is the lowest standard, because it is impossible.

Tech debt is a tool just like financial debt is a tool. Your mortgage debt allows you to do something few people could otherwise. Tech debt is the same. It allows us to gain things. But as we must be reverent of our credit card balance and our mortgage payment, tech debt requires management. So manage it. Don't complain about it. Use it. Quit believing that you're some sort of special person who can avoid it altogether.

## 23. Incentives will win, always

Whatever system we end up working on will have gaps. Despite the terms Brooks uses, many "accidental" gaps don't really happen on accident: Many people quietly understand the cause of problems is that fixing the problem is not incentivized by the working environment --- that anybody with authority does not value the fix over the harm.

The lens of pure incentive and interval drive things much more than any sort of good implementation or anything else you can argue till you're blue in the face about certain technical choices. Many engineers make the mistaken belief that explaining problems in pure technical terms is enough to change minds, simply because the line of explanation is sound from a technical perspective.

When non-technical people don't agree with you, you may believe they don't respect you or they are stupid. Because they continue to live with the problem.

What they are actually doing is responding to incentives and existing within incentives. Their incentives are fundamentally misaligned with yours.

It can be a bitter pill to swallow, but as the craftsperson you are likely going to have to be employed by people who are not craftspeople. We proceed in a delicate alliance.

The craftsperson is usually assessed against something different than the quality of the craft, but rather how the craft comes to benefit the owners of the company. This will constrain the choices that you make, and possibly prevent you from actually spending the requisite time traversing basins of risk, regression, or low output to make the technical change that is The Ideal. The reason that you're stuck building something a certain way isn't because it is not The Ideal, it's because the incentives aren't always aimed at The Ideal. Incentives are larger than you and your own ideas.

This can seem like a depressing world, but I think accepting the difference and divergence actually creates more empathy and less frustration. Software happens to be a vehicle. The company is trying to do something different than what you're trying to do.  You have been hired not because of what the software is but what the software does to make one dollar turn into more dollars.

Very few places in the world with the purpose of simply making software for software's sake. Most of us are navigating the alliance between the craft and what the craft gives us.

Don't forget to zoom-out and realize that the craft is also a means to provide you & your family's own well-being. The enjoyment of craft *can* exist within your job, and I was able to enjoy this for a long time, but oftentimes it is not. Sometimes a job is just a job. Your job may be pretty annoying. It may be pretty frustrating. You may feel like you're always just putting lipstick on a pig or shaving the yak. But accept that by taking the job, you have agreed to live in the world of incentives that interest the owners of the company. (And hopefully this choice has been lucrative --- it is software, after all).

Next time you are preoccupied with your job as you sit down with your family to eat: Stop. Look around. Remember that the reason you do what you do. Your world is bigger than the craft.

## 24. Everything changes, nothing changes

What it means to be a sovereign engineer in the last year has radically changed more so than I've ever seen in the last 25 years. I've gone from being someone typing out the code to now being someone who supervises the set of robots trying to do the same thing, sometimes succeeding, sometimes failing. Sometimes both at the same time.

I think it's a very difficult time to be a software engineer. If you've been doing it as long as I have, the fundamental thing that you enjoy doing in your day, job has probably gone away.

If you are at the start of your career, the skills that you need to develop in order to actually be able to assess and understand whether the robots are going in the right direction or not are may be elusive. Can you learn from the robots? The problem is that the robots can get pretty far without making a mistake, and make it sound pretty good and build something fairly plausible and convincing. That's a very different experience than just getting damn stuck yourself, and I must say there's something nice actually in retrospect about getting damn stuck because it forced you to really think through things in a way that you haven't had to before. I'm concerned that the real force of necessity which drives personal growth becomes diminished for generations to come.

Despite all these things so many things actually don't change at all. Many of the fears people have about the nature of the work fundamentally disappearing are completely overblown.

* *"We won't need junior engineers anymore because AI agents are like junior engineers!"*
* *"We won't need senior engineers anymore because now junior engineers can be like senior engineers since they have AI agents!"*

Neither of these are playing-out. Instead I predict everything just kind of "shifts right" on the output curve. Juniors become a bit more productive, seniors become likely even more productive.

It's kind of like giving everybody electricity. Electricity gave you means that we could start building power tools and it was a lot easier to screw together the framing for a house. But the invention of the power tool did not result in a glut of unemployed architects who designed houses. And despite having electricity, we still have poverty and strife in the world. We just have different tools.

And true builders still need particular tools. If you have used power tools, you know that sometimes you actually do need the screwdriver. You actually do need a precision in in a way that the power tool does not afford you.

Coding was never really your job. Your job is to be a critical thinker. It's actually the thinking manifested the choices and forces that exist in the artifact. That is what you craft is, AI or not. Code shaped by someone thinking critically looks very different than when not.

All that said, there is some tedium in software I don't like that I am happy to hand-off to the LLM. I don't really want to figure out why when I removed a certain C header file that the damn thing won't compile anymore because of some sort of implicit dependency that people have missed for years and years. I hate that crap. Let the LLM do that. Let me think about something else. Thank you, LLM.

I don't think this world is going away. Just as when the search engine arrived, these are like the new search engine. You can work without a search engine, but will limit your experience. What happens 25 years from now? I hope to share more then.

<figure>
<img alt="Prague" src="./img/flowers.jpg" style="max-width: 30rem; width:100%;"/>
</figure>

## 25. Relax

You will never stop having impostor syndrome. I've been doing this for over two decades and I still feel it

But I have good news: Real impostors don't get impostor syndrome. Real impostors know they are impostors rather than fear they are impostors.

So relax. If you have impostor syndrome, it means you aren't an impostor.
