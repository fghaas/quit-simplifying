<!-- .slide: data-background-image="images/openedx.png" data-background-size="contain" -->
# Open edX <!-- .element class="hidden" -->

<!-- Note -->
(Screenshot of <https://openedx.org/>)

Now, with Ansible I could assume that everyone's familiar with the project, with this project I can't, so I'll say a few words to explain what it does.

Open edX is a general-purpose learning platform, not unlike Moodle (though while Moodle is a PHP codebase, Open edX is all Python).

It came out of a platform developed at MIT, with initial collaboration from Harvard and Stanford universities.
In 2013, it was released as an open source project, under the Affero GPL.
So it's been around for more than 10 years.


## edx-platform repo <!-- .element class="hidden" -->
![Screenshot of the openedx/edx-platform repository on GitHub](images/edx-platform.png)

<!-- Note -->
It's widely used not only in academia, but also in business education, and it's something that I use in my day job on a daily basis, as a courseware author, a platform admin, and a developer.

And at its core it's really a Django application, or rather a whole array of Django applications all working together.
It has

* a learning management system (LMS), which is what learners and tutors use,
* a courseware management system, which is what instructional designers and course authors use,
* and lots more adjacent components.

Now, the reason I'm mentioning here is that it's full of great examples how complexity, once it's injected into a project or system, ...


## Complexity does not recede <!-- .element class="hidden" -->

### Takeaway #3

Complexity does not recede.

Abstraction and encapsulation only abstract and encapsulate complexity; they do not reduce it. <!-- .element class="fragment fade-in-then-semi-out" -->

<!-- Note -->
... never recedes.

Complexity is *always* here to stay.

And as a corollary to that, no, you can't make it go away buy abstraction or encapsulation, because all those do is abstract and encapsulate, they don't make it go away.

If you need any illustration of that, look at what bundled subprime mortgage securities did to the global financial markets in 2007. 

Now before I proceed, let's make one thing very clear.
It don't mean to pick on Open edX at all, just like I didn't mean to pick on Ansible earlier.

Even less do I want to insinuate that incompetence is at work in these projects.
I am perfectly convinced that what I'm talking about applies to substantially all collaborative software development projects.

The fact that Ansible and Open edX and so many other projects are open source simply *enables* us to look into them an investigate and analyze these things.

With that said, let me give you one of many, many illustrative examples of the permanence of complexity, from this project.

To wit:


## MongoDB in Open edX <!-- .element class="hidden" -->

We need a shared file system.

We don't want NFS. <!-- .element class="fragment fade-in-then-semi-out" -->

We don't trust GlusterFS. <!-- .element class="fragment fade-in-then-semi-out" -->

We don't trust CephFS. <!-- .element class="fragment fade-in-then-semi-out" -->

Let's use GridFS. <!-- .element class="fragment fade-in-then-semi-out" -->

<!-- Note -->
Courses in Open edX are essentially files, they're imported and exported as tarballs and then need to be stored where the frontend servers can access them.

So this data must be accessible from multiple locations, so there's an architectural requirement for having some kind of shared filesystem somewhere on the backend.

And apparently the developers considered NFS a non-starter at the time (this is something of a folk belief, I need to add, but never mind).

Further, they apparently didn't like GlusterFS, although it was apparently considered at the time (this is hearsay I once got from a developer who was involved at the time).

They also didn't trust CephFS (*that* part is totally reasonable for the 2012/13 time frame, as CephFS didn't become production-ready until 2016).

So, they went with a very strange kinda-sorta-but-not-quite-like-a-filesystem abstraction called GridFS, which is a MongoDB feature.
And with this, they made MongoDB a core component of the platform.


## About two years later

Oh shit!

<!-- Note -->
My personal involvement in Open edX started in 2015, and already at the time there was a sense of "oops this was a mistake; we should get rid of MongoDB".


## About ten years later

We'll get rid of it real soon now.

<!-- Note -->
... and even today, the project hasn't managed to do so.

And it wasn't for lack of trying!
In fact, there was an attempt to refactor this all out of the Open edX core and make it a separate application called Blockstore, which would use S3 storage (or anything compatible with it), and the rest of the Open edX platform would communicate with it solely via a REST API.

And then they found out that *that* came with really bad performance, so they pushed it back *into* the Open edX core. 

So now you've got another thing, *and* MongoDB which you need to track for updates and manage and whatnot, and no net reduction of complexity at all.

No matter what you try, complexity does not recede.

And there's another example.


## edx-platform repo <!-- .element class="hidden" -->
![Screenshot of the openedx/edx-platform repository on GitHub](images/edx-platform.png)

<!-- Note -->
To Open edX's great credit, from the get-go they didn't only open-source the platform code itself.
Like I said, that's a complex array of Django applications, and open-sourcing *just* that would make adoption tremendously difficult.


## edx-configuration repo <!-- .element class="hidden" -->
![Screenshot of the openedx/configuration repository on GitHub](images/edx-configuration.png)

<!-- Note -->
So what they did instead is they also released *all* the tooling required to deploy the platform in an automated fashion.

This was largely based on Ansible, but at least initially they *also* had officially supported CloudFormation templates so you could bootstrap the platform in AWS, and from there do the rest of the configuration with Ansible.

And that allowed you to build an architecture like this:


## Legacy method of hosting Open edX <!-- .element class="hidden" -->
![Diagram explaining the legacy method of hosting Open edX](images/openedx-cluster.svg)

<!-- Note -->
* Automatically deployed 3-node backend with MySQL and MongoDB cluster (and other shared services, like memcached and a Celery backend like Redis).
* As many frontend servers as you liked, all running Django and being essentially stateless.
* Pre-install one frontend VM, take snapshot, refresh snapshot once a month or so.
* Scale frontend in and out by spinning VMs up and down and plugging them in and out of the load balancer.

So this was really neat and cool and useful.
But alas, it was also complicated to use and maintain and somewhat difficult to contribute to.

So again, as with the MongoDB/Blockstore bit, people desired simplification.


<!-- .slide: data-background-image="images/tutor-quickstart.png" data-background-size="contain" -->
## Tutor <!-- .element class="hidden" -->

<!-- Note -->
(Screenshot of <https://docs.tutor.edly.io/quickstart.html>)

And of course, containers make everything simpler, don't they?

So now what happened was a project emerged to run Open edX in Docker containers, ostensibly with a single command.
This project went through a few names, but now it's called *Tutor*.

And of course, just as with the story of Ansible and Puppet, it was *simpler,* initially, because it couldn't do nearly as much as what the Ansible playbooks could do.


<!-- .slide: data-timing="30" data-background-image="images/tutor.png" data-background-size="contain" -->
## Tutor repo <!-- .element class="hidden" -->

<!-- Note -->
(Screenshot of <https://github.com/overhangio/tutor>)

But as soon as it approached something like feature parity, it was of course just as complex as the old bit.

And, what Tutor actually does is it generates docker-compose definitions and Kubernetes manifests for you.
So, of course it's just one more lap around the configuration complexity clock: it's code that generates more code, in a DSL.

But Tutor illustrates another very typical problem in complexity management: making complexity someone else's problem.


<!-- .slide: data-timing="110" -->
## Tutor's feature addition policy repo <!-- .element class="hidden" -->

Let's add only what is useful to a significant fraction of our users! <!-- .element class="fragment fade-in-then-semi-out" -->

... and for the rest, we have plugins. <!-- .element class="fragment fade-in-then-semi-out" -->

<!-- Note -->
In Tutor the lead author maintains a policy that the project will only accept new features [if it benefits "a large proportion (~30-50%) of users"](https://github.com/overhangio/tutor/pull/675#issuecomment-1140919654).
But Tutor has a plugin system that allows you to incorporate any feature you like, without touching the core.

This sounds eminently reasonable: he is trying to make the project no more complex than what's needed for most users.

But, what you end up with is the core of the project being essentially unfit for its purpose without plugins, and the complexity of managing all the plugins and making them work together essentially ends up in the user's (the Open edX platform admin's) lap.

In my experience, it's not uncommon to have to run something like 10 Tutor plugins to achieve some degree feature parity with what the Ansible playbooks offered.

(By the way:
Ansible has this problem too, now that it has split a lot of functionality into collections.
Which we have to manage separately from Ansible Core.)

So that leads us to our next takeaway:


<!-- .slide: data-timing="90" -->
## Pushing complexity downstream does not reduce it. <!-- .element class="hidden" -->

### Takeaway #4
Pushing complexity downstream does not reduce it.

It only makes it someone else's problem.

<!-- Note -->
Now, sure, we would prefer if things were simple, or simpler, for whoever *builds* something, too — not just for whoever *uses* that thing.

But we should keep in mind that the ratio of developers to users is... what?
1:100? 1:1000? 1:10000?

Examples: 
* For Kubernetes the number of code contributors is on the order of 10,000, and there are [a few million Kubernetes clusters deployed worldwide](https://cloudnativenow.com/topics/how-many-kubernetes-clusters-exist-today/). 
  It's difficult to estimate how many clusters there are per admin, and how many admins per cluster.
  So its upstream-to-downstream ratio is probably in the 1:1000 range.
* `curl` in contrast has on the order of 1,000 committers, and it *definitely* has way more than a billion users.
  So there, it's probably a ratio of 1:1000000 or higher.

So making things simpler for some number of people, while making it more difficult for *orders of magnitude* **more** people, is not a net win and not a reduction in complexity, either. 


## So what do we do?

<!-- Note -->
So what can we do, really, to simplify?
To reduce complexity?

Now I hate to bust your bubble, but the answer is probably: nothing.

And I don't say that lightly, but I have physics on my side to back me up.
