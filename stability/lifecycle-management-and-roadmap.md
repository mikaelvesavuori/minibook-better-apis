---
description: TODO
---

# 🗺 Lifecycle management and roadmap

We can follow the convention where we divide an API's lifecycle into `design`, `lifetime`, `sunset`, and `deprecation` phases.

{% hint style="info" %}

Refer to the pattern [Aggressive Obsolescence](https://microservice-api-patterns.org/patterns/evolution/AggressiveObsolescence.html) and articles by [Nordic APIs](https://nordicapis.com/how-to-smartly-sunset-and-deprecate-apis/) and [Stoplight](https://blog.stoplight.io/deprecating-api-endpoints).

{% endhint %}

Beyond these, we add the notion of being `removed` which is the point at which a given feature has been completely purged from the source code.

**🎯 Example**: We'd keep a roadmap like the below in our docs. Imagine that today is 25 November 2021 and see below what our code base represents:

| Feature                                   | Group/Flag            | Beta start      | Stable          | Sunset         | Deprecated       | Removed         |
| ----------------------------------------- | --------------------- | --------------- | --------------- | -------------- | ---------------- | --------------- |
| Use hardcoded response                    | N/A                   | 1 August 2021   | 8 August 2021   | 1 October 2021 | 15 November 2021 | 15 January 2022 |
| Use RandomUser instead of JSONPlaceholder | Group `devNewFeature` | 1 November 2021 | 15 January 2022 | N/A            | N/A              | N/A             |

This gives a clear visibility into the plans and current actions affecting your API. More and more companies and products are using _public roadmaps_ (see for example [GitHub's public roadmap](https://github.com/github/roadmap)). At any rate, ensuring that other enterprise teams and similar stakeholders know what you are doing should be considered a bare minimum.
