---
description: Get ready for Friday deploys with canary deployments.
---

# 🦺 Canary deployment

The _traditional_ way to deploy software is as one huge chunk that becomes instantly activated whenever it's deployed to a machine. If the code was a complete failure, then you end up having zero time to verify and correct this before the failure is apparent to users.

**This notion is what makes managers ask for counter-intuitive things like code freeze and all-hands-on-deck deployments. This is dumb and wrong and helps no-one. Let's forever end those days!**

Matt Casperson, writing for The New Stack, deftly portrays the journey that many are now making when it comes to finding a new "truth" when it comes to testing best practices:

> [...] What I really wanted to do was leverage the existing microservice stack deployed to a shared environment while locally running the one microservice I was tweaking and debugging. This process would remove the need to reimplement live integrations for the sake of isolated local development, which was appealing because these live integrations would be the first things to be replaced with test doubles in any automated testing anyway. It would also create the tight feedback loop between the code I was working on and the external platforms that validated the output, which was necessary for the kind of “Oops, I used the wrong quotes, let me fix that” workflow I found myself in.
>
> My Googling led me to [“Why We Leverage Multi-tenancy in Uber’s Microservice Architecture"](https://eng.uber.com/multitenancy-microservice-architecture/), which provides a fascinating insight into how Uber has evolved its microservice testing strategies.
>
> The post describes parallel testing, which involves creating a complete test environment isolated from the production environment. I suspect most development teams are familiar with test environments. However, the post goes on to highlight the limitations of a test environment, including additional hardware costs, synchronization issues, unreliable testing and inaccurate capacity testing.
>
> The alternative is **testing in production**. The post identifies the requirements to support this kind of testing:
>
> There are two basic requirements that emerge from testing in production, which also form the basis of multitenant architecture:
>
> - Traffic Routing: Being able to route traffic based on the kind of traffic flowing through the stack.
> - Isolation: Being able to reliably isolate resources between testing and production, thereby causing no side effects in business-critical microservices.

— Source: [Embracing Testing in Production](https://thenewstack.io/embracing-testing-in-production/)

{% hint style="success" %}
See these brilliant articles for more justification and why this is important to understand:

- [Charity Majors: I test in prod](https://increment.com/testing/i-test-in-production/)
- [Cindy Sridharan: Testing in Production, the safe way](https://copyconstruct.medium.com/testing-in-production-the-safe-way-18ca102d0ef1)
- [Heidi Waterhouse: Testing in Production to Stay Safe and Sensible](https://launchdarkly.com/blog/testing-in-production-for-safety-and-sanity/)
  {% endhint %}

OK, so what can _we_ do about it? In `serverless.yml` at line \~85, you'll see `type: AllAtOnce`.

{% code title="serverless.yml" %}

```yml
FakeUser:
  [...]
  deploymentSettings:
    type: AllAtOnce
    alias: Live
    alarms:
      - FakeUserCanaryCheckAlarm
```

{% endcode %}

This means that we get a classic `deploy === release` pattern. When the deployment is done, the new function version is immediately active with a clear cut-off between the previous and the current (new) version.

## How we can do better

There are considerations and problems with this approach. In our AWS circumstances, running on Lambda, we won't face downtime while the instance switches over, and even a half-good PaaS solution won't create massive headaches either.

However, we should primarily concern ourselves by considering application-level issues, rather than low-level technical issues (much of which is managed in a, well, _managed_ service). So when that "all at once" deployment is done, if something problematic surfaces that was not caught by testing, there is no obvious way how to proceed. Rollback? Roll forward? Hotfix, yay or nay? Sarcastic note: It's hardly unheard of that even manual testing and code freezes slip up.

Let's be honest: **Shit happens anyway**.

![Words to live by, as told by Mike Tyson](../img/tyson.jpg)

Instead of being overly defensive, let's simply embrace the uncertainty, as it's already there anyway.

Using a [canary release](https://martinfowler.com/bliki/CanaryRelease.html) is one way to get those [unknown unknown effects](https://en.wikipedia.org/wiki/Cynefin_framework) happening with real production traffic in a safe and controlled manner. This is where the (sometimes misunderstood) concept [testing-in-production](https://increment.com/testing/i-test-in-production/) really kicks in—trying to answer questions no staging environment or typical test can address. Like a canary in the mines of old, our canary will die if something is wrong, effectively stopping our roll-out.

**🎯 Example**: Setting the line to `type: Canary10Percent5Minutes` makes the deployment happen through AWS CodeDeploy in a more controlled, bi-directed fashion:

- 90% of the traffic will pass to whatever function version that was already deployed and active...
- ...while the remaining 10% of traffic will be directed to the "canary" version of the function.
- The alarm configuration (defined on lines 75-83) looks for a static value of 3 or more errors on the function (I assume all versions here?) during the last 60-second window.
- After 5 minutes, given nothing has fired the alarm, then the new version takes all of the traffic.

{% code title="serverless.yml" %}

```yml
FakeUser:
  [...]
  alarms:
    - name: CanaryCheck
      namespace: 'AWS/Lambda'
      metric: Errors
      threshold: 3
      statistic: Sum
      period: 60
      evaluationPeriods: 1
      comparisonOperator: GreaterThanOrEqualToThreshold
  deploymentSettings:
    type: Canary10Percent5Minutes
    alias: Live
    alarms:
      - FakeUserCanaryCheckAlarm
```

{% endcode %}

You can either manually send "error traffic" with the `erroruser@company.com` Authorization header, or use the AWS CLI to manually toggle the alarm state. See [AWS docs for how to set the alarm state](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudwatch/set-alarm-state.html), similar to:

```bash
aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"
```

{% hint style="info" %}
Use the above with the `OK` state value to reset the alarm when done.
{% endhint %}

This specific solution is rudimentary, but indicative enough of how a canary solution might begin to look. I highly recommend using a deployment strategy other than the primitive "all-at-once" variety.

{% hint style="info" %}
See this article at [Google Cloud Platform for more information on deployment and test strategies](https://cloud.google.com/architecture/application-deployment-and-testing-strategies).
{% endhint %}
