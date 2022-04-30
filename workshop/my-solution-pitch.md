---
description: TODO
---

# My solution pitch

_**Note**: As the writer of this somewhat elaborate project, yes, it absolutely took more than 2 hours to make all of this! But in fact, the actual implementation and diagramming probably were not much more than that._

We are going to cut down on environment sprawl by using a **single, dynamic environment** instead of multiple hardware-segregated environments (like `dev`, `staging`, `prod` setups) to deliver our stable, qualitative, and observable application.

The only other environment we'll use is a non-user-facing **CI environment for non-production testing**.

We'll use **feature toggles** and **canary releases** to do this _safely_ in our single production environment, now being able to separate a (business-oriented) release, from a mere technical deployment.

If these sound like [**testing-in-production**](https://launchdarkly.com/blog/testing-in-production-for-safety-and-sanity/) strategies, then yep, they are!

To ensure functionality, we'll use **contract testing** and unit/integration/smoke/load testing to verify functionality across the time continuum: During development, before a deployment, after a deployment, and (optionally) under the application's lifetime, using **synthetic monitoring**.

The application will exhibit its dynamic nature by:

- Being able to run different sets of functionality based on if you are a "trusted" user or deliver a default feature set if you are not trusted.
- We'll be able to switch to a new implementation of an external dependency and verify that it works, without breaking the original functionality.
- We can roll out the application over a time window, and stop delivering it (and rollback) if we encounter errors.
- ...and we top it off by using built-in observability in AWS to monitor our application and become alerted if something severe happens.

You can read more about the implementation patterns, and how they are organized into three areas (quality, stability, observability), further down.

## Weighing the negatives between a hardware- or a software-oriented approach

### 🖥️ The drawbacks of hardware-segregated environments

- You start expecting environmental parity between any other systems
- You start expecting that all systems are in similar, co-deployed stages
- The implicit reasoning starts becoming that you should/can only have a very low degree of variability/dynamism in configuration
- As a consequence, such "pre-baked" configuration tests may start becoming large-scale blocking, manual tests
- There may be significant cost overhead with a higher count of static environments
- There is most likely a significant complexity overhead with a higher count of static environments

### 🧑‍💻 The drawbacks of a software-defined, dynamic environment

- Can add complexity
- Will become harder to work with, in the local scope (i.e. the actual code), and more so if there are many branches
- Requires some degree of cleanliness and pruning (governance even) to control, so things don't grow out of hand

### 🎨 Yes, you can mix these patterns with a hardware-separated environment!

You can absolutely use the patterns seen in this project in a more "traditional" hardware-separated environment. However, the benefits become more pronounced as you also shed some of the overhead and weight of classical environments.
