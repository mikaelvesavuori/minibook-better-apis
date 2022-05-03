# Introduction

**"Better APIs: Quality, Stability, Observability" is a practical guide to how you can improve an API, combined with an example project (available on [GitHub](https://github.com/mikaelvesavuori/better-apis-workshop)) and an assignment of sorts, if you'd want to try your hands at making a rock-solid, production-grade API.**

Writing and maintaining APIs can be hard. While the cloud, serverless, and the microservices revolution made it easier and more convenient to set an API skeleton up, age-old issues like software quality ([SOLID](https://stackoverflow.blog/2021/11/01/why-solid-principles-are-still-the-foundation-for-modern-software-architecture/), etc) and understanding the needs of the API consumers still persist.

Werner Vogels, legendary CTO of Amazon, [states his API rules like this](https://www.youtube.com/watch?app=desktop&v=8_Xs8Ik0h1w):

```
1. APIs are Forever
2. Never Break Backward Compatibility
3. Work Backwards from Customer Use Cases
4. Create APIs That are Self Describing and Have a Clear, Specific Purpose
5. Create APIs with Explicit and Well-Documented Failure Modes
6. Avoid Leaking Implementation Details at All Costs
```

In practice, how can we begin moving towards those ideals?

## What will you learn?

This mini-book presents an [application](https://github.com/mikaelvesavuori/better-apis-workshop) and a made-up (but "real-ish") scenario that, taken together, practically demonstrate a range of techniques or methods, patterns, implementations, as well as tools, that all help enhance quality, stability, and observability of applications:

**Quality** means our applications are well-built, functional, safe and secure, maintainable, and are built to high standards.

**Stability** means that our application can withstand external pressure and internal change, without failing at predictably providing its key business values.

**Observability** means that we can understand, from the outputs of our application, what it is doing and if it is behaving well. Or as [Charity Majors writes](https://twitter.com/mipsytipsy/status/1305398051842871297):

> 📉 Monitoring is for running and understanding other people's code (aka "your infrastructure").
>
> 📈 Observability is for running and understanding _your_ code -- the code you write, change and ship every day; the code that solves your core business problems.

Of the three above concepts, _stability_ is the most misunderstood one, and it will be the biggest and most pronounced component here. It will be impossible to reach what Vogels is pointing to, without addressing the need for stability.

{% hint style="info" %}

**Caveat**: No single example project or book can fully encompass all details involved in such a complex territory as this, but at least I will give it a try!

{% endhint %}

## Why should you care?

You will be especially interested in this project if you have ever been involved in situations like the ones below, and want to have ideas for how to address them:

- You inherited something that is "impossible to work with or understand"
- Your team was unable to deliver new features because changes would mean breaking them for someone else
- You built something but don't know who your consumers are
- You document in Confluence or Sharepoint or something like that. If your document at all. You don't, since there is no allotted time for it. OK so maybe the API sometimes, but it's never up to date, really. To be honest, you tell people "ask me if you have questions, don't trust the docs".
- You looked up what [DORA metrics](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) are and laughed out the words "yeah not my company no sirree never"
- You say that you are "autonomous" but someone always keeps freezing deployments to the shared staging environment so the only actual autonomous thing is `localhost`
- You say that you "implemented" continuous delivery, but it's still too painful to integrate and release without a gatekeeper and crashing a hundred systems
- You have heard "we cannot have any confidence in our systems if we don't do real, manual scheduled all-hands-on-deck testing with frozen versions of all external systems" so many times you actually have started to believe that ludicrous statement
- You wonder if things would be better with more infra and more environments, but start having nightmares when you do some back-of-the-napkin math on how many you'd need, not to mention the burden of supporting them logically

There are a million more of those, but you get the point; It's a dark and strange place to be in.

![Giovanni Battista Piranesi - Imaginary Prisons (1745-1761)](/img/piranesi.jpg)

> _[Giovanni Battista Piranesi - Imaginary Prisons (1745-1761)](https://en.wikipedia.org/wiki/Imaginary_Prisons)_

While it's fun to throw around the old Piranesi drawing or [Happiness in Slavery](https://imvdb.com/video/nine-inch-nails/happiness-in-slavery) video as self-deprecating memes, I think it is Escher who brilliantly captures the "positive" side of this mess—That things become very, very strange when multiple realities and gravities are seen _together, at once_. Even if one "reality" is perfectly stable and sound, the work of architects is to support all the realities that need to work together.

![M.C. Escher - Relativity (1953)](/img/escher.jpg)

> _[M.C. Escher - Relativity (1953)](<https://en.wikipedia.org/wiki/Relativity_(M._C.\_Escher)>)_

So: We need to get back some of that control so the totality does not look more like chaos than whatever it is that we are attempting to do.

At the end of the day, our work is about supporting the shared goals (business, organization, or personal goals, if it's a pet project) and letting us stay safe, sound, and happy working professionals while doing so.

## Prior art

You might be interested in [microservices-testing-workshop](https://github.com/mikaelvesavuori/microservices-testing-workshop) for a similar "workshop + demo" approach but focusing on testing an event-driven serverless application.

There's also a previous piece of work you can look at: [multicloud-serverless-canary](https://github.com/mikaelvesavuori/multicloud-serverless-canary), which might pique your interest if you want to see more on the Azure and GCP side of CI and canaries.
