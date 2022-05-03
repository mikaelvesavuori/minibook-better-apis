---
description: Make it clear what you are doing and when you are doing it.
---

# 🗺 Lifecycle management and roadmap

More and more companies and products are using _public roadmaps_ (see for example [GitHub's public roadmap](https://github.com/github/roadmap)). Public or not, ensuring that other enterprise teams and similar stakeholders know what you are doing should be considered a _bare minimum_.

We can follow a basic convention where we divide an API's lifecycle into `design`, `lifetime`, `sunset`, and `deprecation` phases.

{% hint style="info" %}
Refer to the pattern [Aggressive Obsolescence](https://microservice-api-patterns.org/patterns/evolution/AggressiveObsolescence.html) and articles by [Nordic APIs](https://nordicapis.com/how-to-smartly-sunset-and-deprecate-apis/) and [Stoplight](https://blog.stoplight.io/deprecating-api-endpoints).
{% endhint %}

Beyond these, we add the notion of being `removed` which is the point at which a given feature has been completely purged from the source code.

**🎯 Example**: We'd keep a roadmap like the below in our docs. Imagine that today is 25 November 2021 and see below what our codebase would represent at that point in time:

| **Feature**                               | **Group/Flag**        | **Beta start**  | **Stable**      | **Sunset**     | **Deprecated**   | **Removed**     |
| ----------------------------------------- | --------------------- | --------------- | --------------- | -------------- | ---------------- | --------------- |
| Use hardcoded response                    | N/A                   | 1 August 2021   | 8 August 2021   | 1 October 2021 | 15 November 2021 | 15 January 2022 |
| Use RandomUser instead of JSONPlaceholder | Group `devNewFeature` | 1 November 2021 | 15 January 2022 | N/A            | N/A              | N/A             |

This takes only a few minutes to set up, but already gives others clear visibility into the plans and current actions affecting your API.
