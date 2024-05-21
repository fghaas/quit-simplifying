# Ansible

<!-- Note -->
This is a Python conference!
So, considering that it's a Python projects, it's probably safe to assume that everyone around here knows Ansible, at least to some degree.

* Who knows Ansible?
* Who has used Ansible to deploy or manage production systems?
* Who has reported an Ansible issue or feature request?
* Who has contributed to Ansible?


<!-- .slide: data-timing="15" data-background-image="images/ansible-github.png" data-background-size="contain" -->
## Ansible repository on GitHub <!-- .element class="hidden" -->

<!-- Note -->
(Screenshot of: <https://github.com/ansible/ansible>)

You are most probably familiar with this.

This is the Ansible repository on GitHub, whose commit history goes all the way back to 2012-02-23.

```patch
commit f31421576b00f0b167cdbe61217c31c21a41ac02
Author: Michael DeHaan <michael.dehaan@gmail.com>
Date:   Thu Feb 23 14:17:24 2012 -0500

Genesis.
```


<!-- .slide: data-timing="10" data-background-image="images/ansible-citation-needed.png" data-background-size="contain" -->
## Ansible, citation needed <!-- .element class="hidden" -->

<!-- Note -->
(Cropped screenshot from: <https://en.wikipedia.org/wiki/Ansible_(software)>)

Curiously, Wikipedia [has Ansible's initial release](https://en.wikipedia.org/wiki/Ansible_(software)) as having occurred on 2012-02-20, "[citation needed]"


<!-- .slide: data-timing="30" data-background-image="images/ansible-github.png" data-background-size="contain" -->
## Ansible repository on GitHub (2) <!-- .element class="hidden" -->

<!-- Note -->
So anyway, back to the GitHub repo.

There's something up here in the repository description that **I think** has been mostly unchanged since 2012.
At least I remember it to have been there when I first started with Ansible, and I think I started pretty early on.

Here's what it says:


<!-- .slide: data-timing="30" -->
## Ansible repository description <!-- .element class="hidden" -->
Ansible is a radically simple IT automation platform that makes your applications and systems easier to deploy and maintain.

Automate everything from code deployment to network configuration to cloud management, in a language that approaches plain English, using SSH, with no agents to install on remote systems.

<!-- Note -->
So the second sentence I believe is more recent, but the first one is the one I think I remember as having been there from the get-go.


<!-- .slide: data-timing="5" -->
## "Radically simple" automation platform <!-- .element class="hidden" -->
Ansible is a **radically simple** IT automation platform that makes your applications and systems easier to deploy and maintain.


<!-- .slide: data-timing="5" -->
## "Radically simple" <!-- .element class="hidden" -->
**radically simple**


<!-- .slide: data-timing="30" data-background-image="images/srsly.jpg" data-background-size="contain" -->
## Srsly. <!-- .element class="hidden" -->

<!-- Note -->
Would you actually describe Ansible to someone who is just starting out as "simple"?

I think not.

But where does that come from?

Well first of all whether or not something is "simple" depends on: compared to what?

And in 2012, when Ansible started, the thing you would compare it against was the incumbent in the server automation space, which was of course Puppet.
And Puppet had been around since 2005.


## 2012

|          | Puppet | Ansible    |
|----------|--------|------------|
| Simple   | Nah    | Yeah       |
| Age      | 7      | 0          |
| Could Do | A Lot  | A Lot Less |

<!-- Note -->
So naturally, Ansible was "simpler" than Puppet, just because there was a jolly good lot of things you could do with Puppet, and a lot *less* you could do with Ansible.

Put simply:


## Takeaway #1

New systems frequently feel simpler just because they can do less than established ones.

<!-- Note -->
That's largely due to the fact that for about 90% of software systems, you can probably design a replacement in 6 weeks, bang out a first release in 6 months, and then spend 6 years ironing out kinks and covering edge cases and corner cases.

And perhaps for another 5 percent of systems it's 10 rather than 6.

But, as a system grows more features and capabilities, it's subject to a rather interesting phenomenon, which also contributes to growth in complexity.
