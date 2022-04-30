---
description: TODO
---

# My solution pitch

_**Note**: As the writer of this somewhat elaborate project, yes, it certainly took me more than 2 hours to make all of this! But in fact, the actual implementation and diagramming probably were not much more than that._

We're going to cut down on environment sprawl by using a **single, dynamic environment** instead of multiple hardware-segregated environments (like `dev`, `staging`, `prod` setups) to deliver our stable, qualitative, and observable application.

The only other environment we'll use is a non-user-facing **CI environment for non-production testing**.

We'll use **feature toggles** and **canary releases** to do this _safely_ in our single production environment, now being able to separate a (business-oriented) release from a mere technical deployment.

If these sound like [**testing-in-production**](https://launchdarkly.com/blog/testing-in-production-for-safety-and-sanity/) strategies then–yep, they are!

To ensure functionality, we'll use **contract testing** and unit/integration/smoke/load testing to verify functionality across the time continuum: During development, before a deployment, after a deployment, and (optionally) under the application's lifetime, using **synthetic monitoring**.

The application will exhibit its dynamic nature by:

- Being able to run different sets of functionality based on if you are a "trusted" user or deliver a default feature set if you are not trusted.
- We'll be able to switch to a new implementation of an external dependency and verify that it works without breaking the original functionality.
- We can roll out the application over a period of time, and stop delivering it (and rollback) if we encounter errors.
- We top it off by using built-in observability in AWS to monitor our application and become alerted if something severe happens.

Later in this guide, you can read more about the implementation patterns and how they are organized into three areas (quality, stability, observability).
