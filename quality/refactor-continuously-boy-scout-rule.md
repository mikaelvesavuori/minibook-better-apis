---
description: TODO
---

# ⚒ Refactor continuously ("boy scout rule")

First, what is _refactoring_ exactly? [In the words of Martin Fowler](https://refactoring.com), who wrote [one of the definitive books on the subject](https://www.amazon.com/Refactoring-Improving-Existing-Addison-Wesley-Signature/dp/0134757599),

> Refactoring is a disciplined technique for restructuring an existing body of code, altering its internal structure without changing its external behavior.
>
> Its heart is a series of small behavior preserving transformations. Each transformation (called a "refactoring") does little, but a sequence of these transformations can produce a significant restructuring. Since each refactoring is small, it's less likely to go wrong. The system is kept fully working after each refactoring, reducing the chances that a system can get seriously broken during the restructuring.

It takes perseverance, good communication, and business folks that truly understand software to get dedicated time for improving the solutions we work on. Most people will unfortunately not work in teams with dedicated refactoring time. What to do, then?

Rather than think of "change" as a drastic, singular, large-scale, and long-term event, it's better to see change as a stream of small, manageable events that we can shape. Robert Martin wrote about the notion that every change in a codebase should also include some form of improvement in his classic book, [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882/): The _boy scout rule_ which in the engineering context means "always leave the code better than you found it".

{% hint style='info' %}

Read a [short summary here](https://matheus.ro/2017/12/11/clean-code-boy-scout-rule/) and a [longer article on continuous refactoring here](https://www.codit.eu/blog/continuous-refactoring/).

{% endhint %}

Moreover, the "boy scout rule" is definitely colored by other (at the time) contemporary management ideas like [kaizen](https://en.wikipedia.org/wiki/Kaizen) that work well in agile/lean contexts.

But what about our early work, when we are just starting on a new feature or product? In that case I personally love, and truly resonate with, [Martin's notion to start with "degenerate tests"](https://blog.cleancoder.com/uncle-bob/2019/06/08/TestsAndTypes.html) (also in the book, [Clean Craftsmanship](https://www.amazon.com/Clean-Craftsmanship-Disciplines-Standards-Ethics/dp/013691571X)):

> We begin with the degenerate tests. We return an empty list if the input list is empty, or if the number of requested elements is zero. [...] Note that I am following the rule of gradually increasing complexity. Rather than worrying about the whole problem of random selection, I’m first focusing on tests that describe the periphery of the problem.
>
> We call this: "Don’t go for the Gold". Gradually increase the complexity of your tests by staying away from the center of the algorithm for as long as possible. Deal with the degenerate, trivial, and simple administrative tasks first.

Practically it means that we start coding and testing from a perspective where the boundary functionality _works_, but has no complexity or elegance. Then, bit by bit, we add necessary complexity (dealing with harder problems more realistically) while subtracting unintended complexity (using SOLID principles etc.) so we end up with something that worked early on, yet _evolved_ into a coherent and good piece of work.

It would be wrong to assume that all code necessarily has this evolutionary spiral into something better—in fact, I think it's correct to say that most code, unfortunately, grows worse. Again, we must remember that all code is a liability. It is the efficient pruning and nurturing, methodically done, that allows code to actually grow better with time. Refactoring is the key to this, and we should do it as early and often as possible, ideally within minutes of our first working code.

**🎯 Example**: Hard to point to something "post-fact", but every single bit has been continuously enhanced and refactored (sometimes removed) since starting this project. This very book has, as well, going from a README file to become a full Gitbook project!

{% hint style='info' %}

Go ahead and check out [Refactoring.guru](https://refactoring.guru) for lots of ways to approach making practical code improvements. Also, see the [reference list](tips-and-references.md) for even more materials.

{% endhint %}
