# What is this?

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

This mini-book presents an [application](https://github.com/mikaelvesavuori/better-apis-workshop) and a made-up (but "real-ish") scenario that, taken together, practically demonstrate a range of techniques or methods, patterns, implementations, as well as tools, that all help enhance quality, stability, and observability of applications:

**Quality** means our applications are well-built, functional, safe and secure, maintainable, and are built to high standards.

**Stability** means that our application can withstand external pressure and internal change, without failing at predictably providing its key business values.

**Observability** means that we can understand, from the outputs of our application, what it is doing and if it is behaving well. Or as [Charity Majors writes](https://twitter.com/mipsytipsy/status/1305398051842871297):

> 📉 Monitoring is for running and understanding other people's code (aka "your infrastructure").
>
> 📈 Observability is for running and understanding _your_ code -- the code you write, change and ship every day; the code that solves your core business problems.

Of the three above concepts, _stability_ is the most misunderstood one, and it will be the biggest and most pronounced component here. It will be impossible to reach what Vogels is pointing to, without addressing the need for stability.

{% hint style='info' %}

**Caveat**: No single example can fully encompass all details involved in such a complex territory as this, but at least I will give it a try!

{% endhint %}
