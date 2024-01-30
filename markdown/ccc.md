# Configuration Complexity <!-- .element class="hidden" -->

Mike Hadlow, *The Configuration Complexity Clock*

http://mikehadlow.blogspot.com/2012/05/configuration-complexity-clock.html

<!-- Note -->
What I'm talking about is the Configuration Complexity Clock.

It's from an article written by [Mike Hadlow](https://mikehadlow.com/), who happens to be a C#/.NET developer living in Brighton, in 2012.

And it's one of of those things that every software engineer and engineering manager *should* have read, but few actually have --- not unlike Fred Brooks' [Mythical Man-Month](https://en.wikipedia.org/wiki/The_Mythical_Man-Month), in a way.

Here's what it says.


<!-- .slide: data-timing="120" -->
## Configuration Complexity Clock <!-- .element class="hidden" -->
![Mike Hadlow's Configuration Complexity Clock](images/ccc.svg)

<!-- Note -->
The idea is that we think of the development state of a system as the face of a clock.

Initially, the system is at noon (or midnight, if you like), where everything is hard coded.
To make the system behave differently, we have to change the code.

Then, as the system evolves, we get to the point of saying some kind of ability to configure things would be good, and we add configuration values.
Those could be variables in a config file, or options and arguments in a CLI, or even environment variables for a container (the envars weren't in the original article as that wasn't particularly common at the time, but the same principle applies).
That's 3 o'clock.

Then we might begin to add relationships between our config options, and it eventually becomes a rules engine, where for example a subcommand behaves differently based on a global mode switch or config option.
That's 6 o'clock.

Then all of that eventually morphs into a domain-specific language (DSL), and we hit 9 o'clock.

And eventually we realise that managing our DSL by hand is too unwieldy, and we write some code to generate that DSL for you.

And we're back at 12 o'clock.

So all of those things that we've done ostensibly to keep things manageable as the system evolves and grows more features, really just get us right back to the starting point.


<!-- .slide: data-timing="25" -->
## Mike Hadlow Quote <!-- .element class="hidden" -->
> To be honest, I’ve never seen an organisation go all the way around the clock, but I’ve seen plenty that have got to 5, 6, or 7 and feel considerable pain.

--- Mike Hadlow, 2012


<!-- .slide: data-timing="10" data-background-image="images/sweet-summer-child.jpeg" data-background-size="contain" -->
## "Oh, my sweet summer child" <!-- .element class="hidden" -->

"Oh, my sweet summer child" (famous quote from Game of Thrones) <!-- .element class="hidden" -->


<!-- .slide: data-timing="10" data-background-image="https://upload.wikimedia.org/wikipedia/en/1/16/BladeRunnerRoyBattySpeech.jpeg" data-background-size="contain" -->
## Tears in Rain <!-- .element class="hidden" -->

"Tears in Rain" speech by Roy Batty (Rutger Hauer) from the film Blade Runner, 1982 <!-- .element class="hidden" -->


<!-- .slide: data-timing="90" -->
## Configuration Complexity Clock for Ansible (1) <!-- .element class="hidden" -->
![The Configuration Complexity Clock, with labels for Ansible](images/ccc-ansible-1.svg)

<!-- Note -->
Ansible is fascinating because it gives you the Configuration Complexity Clock not just in the evolution of Ansible, but also in the typical evolution of *using* Ansible:

As an Ansible novice, you start at 12 o'clock with writing a playbook.

Then you realise that it would be nice to make some values configurable, so you define some and add a vars file.
3 o'clock.

Then you create your first inventory, aggregating hosts to groups and applying roles and setting variables for *them*, and you've built a crude rules engine. 6 o'clock.

Then you notice you can write your own roles, and they can have defaults, and in both the roles and the defaults you can use Jinja2 templates, and now you operate in a very peculiar, loosely defined, but clearly *domain-specific* language, namely Ansible's weird mix of YAML and Jinja2 with some straight-up Python sprinkled in.
9 o'clock.

And then you figure out hold on, I can discover most of what I need for my inventory automagically, at which point I don't even need manually maintained `host_vars` and `group_vars` anymore, ...


## Configuration Complexity Clock for Ansible (2) <!-- .element class="hidden" -->
![The Configuration Complexity Clock for Ansible, having gone all around](images/ccc-ansible-2.svg)

<!-- Note -->
... so you end up writing a dynamic inventory driver, and it's noon again.

And of course the cycle doesn't stop there, because now your *inventory driver* grows configuration options, etc. etc. ad infinitum.


## Other Configuration Complexity Clock examples <!-- .element class="hidden" -->

Terraform → Terragrunt <!-- .element class="fragment fade-in-then-semi-out" -->

Kubernetes manifests → Helm charts <!-- .element class="fragment fade-in-then-semi-out" -->

<!-- Note -->
And just so I'm abundantly clear here, this is by no means limited to Ansible.

The Configuration Complexity Clock is everywhere.

Terraform, for example, beelined straight to defining a DSL (perhaps in the hope that that would inoculate them from going further round the clock).
But of course people got tired of maintaining HCL by hand, and started Terragrunt.

Kubernetes manifests are another example, with people getting sick of hacking them by hand, and using Helm to generate them.

There's just no escaping the clock.


## Quit pretending you can escape <!-- .element class="hidden" -->

### Takeaway #2

**Quit pretending you can escape the Configuration Complexity Clock.**

That next iteration you think will solve your complexity problem, won't.
