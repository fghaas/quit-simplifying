## Three theories to explain the universe

General relativity <!-- .element class="fragment fade-in-then-semi-out" -->

Quantum mechanics <!-- .element class="fragment fade-in-then-semi-out" -->

Thermodynamics <!-- .element class="fragment fade-in-then-semi-out" -->

<!-- Note -->
As far as we know, there are three fundamental physical theories that, combined, explain the universe as we know it: 

* general relativity, which explains mass, energy, spacetime, and gravity;
* quantum mechanics and the Standard Model, which explain the other three fundamental forces of nature — electromagnetism, the strong force, and the weak force —, which in turn explains structure, and they nature of atoms, and ultimately chemistry;
* and finally, thermodynamics, which among other things explains heat, and work.

Thermodynamics has a famous Second Law that can be stated in various ways — in one modern and simplified form, we say:


## Second Law of Thermodynamics

The total entropy of a system never decreases.

<!-- Note -->
Now, what does that mean?


## Entropy

The degree to which something is disorderly.

<!-- Note -->
“Entropy,” in this context, is essentially the degree to which the system is disorderly.

More precisely, entropy is a measure of the number of possible configurations of a system, and of course the more *possible* configurations we have in a system, the more *probable* it is that the system is in one that isn't useful, that is, a disorderly one.

In effect, the Second Law states that any system can stay just as orderly as it is now, or it can become more disorderly, but it can never again become as orderly as it once was.

The normal state of the world is that things keep getting more and more disorderly.

There are multiple classic examples of this: 

* you can mix two paints in a bucket but cannot unmix them, 
* you can open a container of gas in a vacuum chamber and the gas will disperse but never go back into the container, 
* you can scramble and cook an egg but never return it to its original protein structure.


## A curious consequence

growth in entropy

=

passage of time

<!-- Note -->
And there's a very curious consequence that arises from this:

The inexorable growth in entropy over time is one of the best explanations we have why time only ever goes in one direction.

You see, since Minkowski we know that space and time are really one and the same thing and can be described as a 4-dimensional spacetime.
And since Einstein we know that all of spacetime is equally influenced by mass, and the principles of general relativity apply to both.

But motion through space is reversible, and motion through time isn't, and that's peculiar.
And the inextricable linkage between growth in entropy is one of the best possible explanations we have for that.

So: reducing entropy --- simplifying things --- is just as realistic as travelling backwards in time.

But!
We can do things to (inadvertently) speed up the growth of entropy, and we can do the opposite to (hopefully deliberately) slow it down.

So what are ways to drive entropy, in the kind of systems we typically manage?


<!-- .slide: data-timing="90" -->
## What drives disorder 

(in software systems)

Adding features without feature flags <!-- .element class="fragment fade-in-then-semi-out" -->

Adding features without test cases <!-- .element class="fragment fade-in-then-semi-out" -->

Adding features without documentation <!-- .element class="fragment fade-in-then-semi-out" -->

Refactoring without feature parity <!-- .element class="fragment fade-in-then-semi-out" -->

Principle of Least Astonishment (POLA) violations <!-- .element class="fragment fade-in-then-semi-out" -->

<!-- Note -->
There's a huge number of ways to drive entropy in a software system.

Of course, in principle *every* addition to a software system increases its entropy: what you add, can go wrong.

But there are some things that are extremely common and that increase entropy much, much more:

* Adding features without gating them behind a feature flag means once the feature is rolled out, you have no way to undo it, and any unexpected fallout from your change is not only here to stay, but continues to accumulate, making the mess harder and harder to fix.

* Adding features without test cases means when something breaks in a later addition, you have no idea what broke, exactly.

* Adding features without documentation means you have no record how the system is *expected* to behave, so you can't even *tell* if something broke or not.

* Refactoring without first achieving feature parity means you're going to run with two or more parallel implementation strands for essentially ever, because you can be damn sure that that *one* thing you could leave out of the refactoring now breaks at least *one* user's workflow.

* And violations of the Principle of Least Astonishment (POLA) --- in other words, the system behaving unintuitively --- will just screw up everything, forever.

So what does this mean for the people responsible for those systems?
What is *their* job?


## A manager's job <!-- .element class="hidden" -->

### Takeaway #5
If you are a responsible for a system, it's your job to slow the growth of disorder in that system.

<!-- Note -->
"System" is deliberately very broad here: a software project, a platform you manage, and even an organizational unit that you are responsible as a manager for. 

You won’t be able to *reduce* disorder, and any attempt to do so pits you against a most fundamental law of physics.

(Laws of physics are like terrorists: you shouldn’t attempt to negotiate with them.) 

But you can do your damndest to slow it down.


## How can we slow the growth of disorder? <!-- .element class="hidden" -->

Docs or it didn't happen <!-- .element class="fragment fade-in-then-semi-out" -->

POLA <!-- .element class="fragment fade-in-then-semi-out" -->

Test cases or it won't merge <!-- .element class="fragment fade-in-then-semi-out" -->

Feature flag or it won't deploy <!-- .element class="fragment fade-in-then-semi-out" -->

Deprecate – Remove – Refactor <!-- .element class="fragment fade-in-then-semi-out" -->

<!-- Note -->
Here's what we can do:

Never land a change without documentation.
Always make sure the documentation update happens simultaneously with the change.
Docs define how the system is supposed to behave.

Always follow the principle of least astonishment: make sure the system behaves in a way that "makes sense".
And no, following item #1 in this list is not a free pass for this:
of course, it is 100 times worse for a feature to behave counter-intuitively and *not* be documented, than to behave counter-intuitively and *be* documented.
But it's 100 times *better* for the system to behave intuitively, and be documented.

Never merge a feature without a corresponding test case.
Always include the test case in the patch that merges the feature.

Never send a new feature to prod without a feature flag.

And for when you decide a particular part of the system needs refactoring, and you think there are some things that the refactor would touch that you actually don't need anymore:
figure out what you think you no longer need, deprecate it, cut a release, *remove it,* cut a release, and *then* refactor what remains *to full feature parity.*

These are all things that any developer, sysadmin, operator can do to stem the growth of entropy in a system.


## Most managers == entropy accelerators

<!-- Note -->
However, many managers are exactly the opposite: they are entropy accelerators; they speed up the growth of disorder in the organization.

I've met one manager who looked at the Slack channel that their team used and found that communications were poor and chaotic, and it was impossible to follow what was happening.

Their solution: more channels.
(More channels that *everyone* who previously followed one channel now had to follow.)

This is not a singular example.
This sort of thing happens all the time.

I posit that this is largely due to an oddly misguided fetish we have for the thing we call "leadership".

People seem to believe that in order to be useful, people must **lead**, they must **do**, they must **direct** others, when in reality there are at least three productive things that people can do when they collaborate.
And one of them is not necessarily better than the others.


## Productive things people can do <!-- .element class="hidden" -->

### Lead,

### Follow,

### Or get out of the way.

(George S. Patton, paraphrased)

<!-- Note -->
Now I chose a paraphrase of that Patton quote because the original, naturally, contains more profanity.

But really, there are three things that people can do in order to be productive, and "people" in this context very much *includes* managers.
And I wish managers would sometimes take option #2 or #3, rather than always default to #1 and stirring the pot, creating ever more disorder.

And I want to also mention *another* thing that people, including but not limited to managers, frequently misunderstand.
