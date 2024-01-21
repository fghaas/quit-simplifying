# Open edX

<!-- Note -->
Now, with Ansible I could assume that everyone's familiar with the project, with this project I can't, so I'll say a few words to explain what it does.

Open edX is a general-purpose learning platform, not unlike Moodle (though while Moodle is a PHP codebase, Open edX is all Python).


## edx-platform repo <!-- .element class="hidden" -->
![Screenshot of the openedx/edx-platform repository on GitHub](images/edx-platform.png)

<!-- Note -->
It came out of a platform developed at MIT, with initial collaboration from Harvard and Stanford universities.
In 2013, it was released as an open source project, under the Affero GPL.
So it's been around for more than 10 years.

It's widely used not only in academia, but also in business education, and it's something that I use in my day job on a daily basis, as a courseware author, a platform admin, and a developer.

And at its core it's really a Django application, or rather a whole array of Django applications all working together.

Now, the reason I'm mentioning here is that it's full of great examples how complexity, once it's injected into a project or system, ...


## Complexity does not recede <!-- .element class="hidden" -->

Complexity does not recede.

<!-- Note -->
... never recedes.

Complexity is *always* here to stay.

Let me give you one of many, many illustrative examples from this project 

To wit:


## MongoDB in Open edX <!-- .element class="hidden" -->

We need a shared file system. <!-- .element class="fragment fade-in-then-semi-out" -->

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


## Tutor repo <!-- .element class="hidden" -->
![Screenshot of the overhangio/tutor repository on GitHub](images/tutor.png)


## Monolith to Microfrontends


MVC to REST+XHR/Fetch

50 MFEs

2 releases per year


## 10-12 years?

"What's a React, grandpa?"
