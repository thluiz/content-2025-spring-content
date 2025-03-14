---
created: 2025-02-05T20:20:23 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: 
---

# 

> ## Excerpt
> Amazing as it may seem after all these years, there are still junior developers in the world.
A few weeks ago at work we had a talk where senior developers (including me) were invited to spend around five minutes each talking about our personal software development philosophies....

---
## Developer philosophy

Amazing as it may seem after all these years, there are still junior developers in the world.

A few weeks ago at work we had a talk where senior developers (including me) were invited to spend around five minutes each talking about our personal software development philosophies. The idea was for us to share our years of experience with our more junior developers.

After the session, I felt that it might be valuable to write my own thoughts up, and add a little more detail. So here we are.

This listing is a little miscellaneous; it isn't intended to be an exhaustive exploration of the way in which I develop software. Also, if you are a senior developer already then obviously you might already be familiar with some of this. Or disagree! Software development is a famously subjective field. See you in the comments.

### Avoid, at all costs, arriving at a scenario where the ground-up rewrite starts to look attractive

It's generally pretty well-understood that the ground-up rewrite can be an attractive and extremely dangerous prospect. The standard advice when it comes to ground-up rewrites is "Don't, ever". But I want to take a step back from that.

By the time the ground-up rewrite starts to seem like a good idea, _avoidable mistakes have already been made_. This is a scenario which you can see coming from a long way out and you can, and must, actively steer away from.

Warning signs to watch for: compounding technical debt. Increasing difficulty in making seemingly simple changes to code. Difficulty in documenting/commenting code. Difficulty in onboarding new developers. Dwindling numbers of people who know how particular areas of the codebase actually work. Bugs nobody understands.

Compounding complexity must be fought at every turn. [Alternate between phases of expansion (new features) and consolidation](https://knowledge.csc.gov.sg/ethos-issue-21/how-to-build-good-software/#:~:text=good%20software%20involves-,alternating%20cycles%20of%20expanding%20and%20reducing%20complexity,-.%20As%20new%20features).

Of course, a ground-up rewrite _can_ actually work. It might even be a better choice that the alternative (persisting with your existing technical debt-laden swamp of code). Equally, it might be that _neither_ choice will work — the project is doomed, and you're just choosing how it dies. The point is that there is inherent risk to this situation... but the situation itself is avoidable, and that risk is avoidable.

### Aim to be 90% done in 50% of the available time

There is a famous adage in software development — actually, thinking about it, it might originate outside of software development — which goes,

> The first 90% of the job takes 90% of the time. The last 10% of the job takes the other 90% of the time.

This is mildly amusing and absolutely factually accurate. Having understood this, it is entirely possible to correct for it.

Writing the code, once, and getting it to work, takes a certain amount of time. Once you have done this, you need to understand that you are about half done. Polishing code up to a suitable level of coherence and maintainability, proper handling of edge cases and failure cases, unit testing, integration testing, usability testing/demos, "last-minute" feature changes, performance, serviceability, documentation... all of these things can take immense amounts of additional time, and they are also part of your job.

Many of these things are _theoretically_ skippable. But in practice when you skip these things you end up with a shoddy, incomplete feature. And nobody is ever going to come back and finish the work "properly" afterwards. There is always more work. Do this three or four more times and you have a shoddy product.

Also, the writing of the code itself will throw up unexpected roadblocks. It is advisable to try to discover these roadblocks as soon as possible.

And if it magically turns out you don't need the extra time you planned for? Great, time to implement some process improvements! Or pay down some technical debt (see above)!

### Automate good practice

Sometimes there's a particular thing which developers on a project should all _start_ doing or _stop_ doing. There's new best practice. There's a new tool we need to use consistently, everywhere; a new mandatory header on every source file; a check everybody has to run; a method which we've collectively decided is no good to use (either an internal method or a third-party API). When this happens, there are two ways to get the developer base as a whole to change its behaviour:

1.  Socialise it. Tell everybody in person, one at a time or at the scrum or at the team meeting. Send out emails. Add the new guidelines to the wiki, or to the repo README, or the pull request remplate. Remind people to read the documentation, over and over. Manually review everybody's changes for oversights, forever. Make sure you never forget! Add checklists, try to train everybody to properly enforce those checklists. Increase the level of mandatory peer review. Remind everybody again. And again...
2.  Automate it.

Add an automated test which fails if the guideline is not followed. Or, if we can't fix everything everywhere all at once, add a [ratchet](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/ratchet). Fail fast, with a polite and instructive warning, if the right thing isn't done, or better yet, automatically fix the problem. In general, enforce best practice mechanically.

Automation isn't a perfect solution or a universal solution or a universally appropriate solution for things like this. There are plenty of softer requirements and abstract technical requirements which can't be automated, and it's possible to get _really_ annoyingly strict by introducing too many _arbitrary_ rules, and motivated developers can usually circumvent automation by various means. But if you find yourself telling people over and over again, "You forgot to do X, please remember to always X", maybe it's time to automate X?

### Think about pathological data

Nobody cares about the golden path. Edge cases are our _entire job_. Think about ways in which things can fail. Think about ways to try to make things break. Code should handle _every_ possibility.

What if the request fails, or stalls forever, or sends back one byte per second for an hour? What if the table you're showing has a million rows? A billion rows? What if the name has a slash in it, or trailing whitespace, or is a megabyte long? I don't believe you when you say that you can prove that that string can't be empty!

### There is usually a simpler way to write it

If you budgeted your time properly (see above), you have time to go back and see if you can do better. _C.f._ the old chess adage, "When you see a good move, look for a better one." And another difficult-to-source quote, "I apologise for writing such a long letter, but I didn't have time to write a short one."

### Write code to be testable

This means well-defined interfaces and minimal side-effects. Code which is proving to be difficult to test is probably not properly encapsulated.

### It is insufficient for code to be provably correct; it should be obviously, visibly, trivially correct

Some code seems to work correctly by accident, because the circumstances which could cause it to receive bad inputs and fail are ruled out by the structure of the other code surrounding it. I dislike this. Although technically the code may be free of bugs, restructuring the other code is now difficult and dangerous.

This is particularly true for security issues, or the theoretical absence of security issues. It doesn't matter that all of the callers to this particular internal function are trustworthy _right now_.

I had one other thing for this list but I don't remember it right now.

### Discussion (6)

#### [2025-02-03 19:31:33](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#komment67a11995522df) by Richard B:

\> And another difficult-to-source quote, "I apologise for writing such a long letter, but I didn't have time to write a short one." Pascal: "Je n'ai fait cette lettre-ci plus longue que parce que je n'ai pas eu le loisir de la faire plus courte."

#### [2025-02-03 19:33:18](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#komment67a119fe9be56) by Richard B:

\> I had one other thing for this list but I don't remember it right now. Always check twice for off by one errors.

#### [2025-02-03 19:59:54](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#komment67a1203a4c223) by tyler:

\> Add the new guidelines to the wiki Yes, it's obviously better to avoid any need for this, but it touches on a point that sometimes needs emphasis: people need to know where to find the information they need, how to use the automated systems, and so on. I'm reminded of a Mitch Hedberg joke: "This shirt is dry-clean-only... which means it's dirty." At one job, our "official" software documentation had to go through a whole approval process, which meant it was outdated. Nobody really wanted to go through the bureaucracy of updating it, or even knew how to do that, so eventually those documents were mostly forgotten about. As for wikis, it would've been nice to have "the" wiki; we had 3 or 4 at first, and nobody would ever update them because nobody knew which was canonical; as with the documents, they thus became a useless and forgotten resource. After too much time, we decided on one wiki, copied stuff there, and told everyone. Then when a junior person found something wasn't explained there, I could write a mail message and paste it into the wiki, or ask them to reverse-engineer and document it. We'd make sure all the relevant documents were linked from there; a page would say who's familiar with which areas; and so on. That was a big improvement. Of course, going back to your point, it's better to have this stuff be accessible: documents stored in source control with the code are usually better than those in a wiki; code comments (within reason) are probably better than separate documents; code that's so obvious that it doesn't need explanation is ideal. Automated testing and building was a whole other problem. A lot of these tests had "weird" environmental requirements such that most programmers had never run \*all\* the tests. Same for the builds; a "desktop build" would never \*quite\* match the official Jenkins builds, if it worked at all. Plus the system would only build what was already committed to the repository, which meant breakage would sometimes be detected too late. The builds were run on "janky" virtual machines (hard-coded passwords etc.), inaccessible to us, managed by an entirely separate team; we didn't fully understand their setup, and they didn't understand our software. One tip: don't use non-trivial Jenkins "build scripts"; it shouldn't be much more than "cd project; ./build-it", with "build-it" being source-controlled like everything else, and the same script that people use at their desks. And if virtual machines are used, make those images available to everybody. I guess it all kind of goes back to the same point: try to avoid "edge cases". Make destroy\_object(foo) work when foo is NULL, rather than expecting callers to check; have a failed create\_object() call destroy\_object() rather than duplicating its logic ("goto fail7"); have developers run the same build and test scripts as the servers; have everyone participate in documentation.

#### [2025-02-03 20:17:15](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#komment67a1244bd4d2c) by Aybri:

There is no tool for changing behavior more effective than inconveniencing people until they memorize the wanted behavior.

#### [2025-02-04 17:01:14](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#komment67a247da73dca) by Adam:

Junior dev here. Thanks for the two cents!

#### [2025-02-04 20:28:42](chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html#komment67a2787af3f2a) by Ian Z:

Actually, I think the chess adage you quote is a \*reaction\* to an advice of the famous Capablanca, who (mythically?) said "If you see a good move, for God's sake, play it!" So there are two sides to this, in chess and in programming :-)
