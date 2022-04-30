---
description: TODO
---

# 🦺 Canary deployment

The traditional way to deploy software is as one big block that becomes instantly activated whenever it's deployed to a machine. If the code was a complete bomb, then you have zero time to verify this. This notion is what makes managers ask for counter-intuitive things like code freeze and all-hands-on-deck deployments. Let's forever end those days!

In `serverless.yml` at line \~85, you'll see `type: AllAtOnce`. This means that we get a classic "deploy === release" pattern. When the deployment is done, the function version is active, with a clear cut-off between the previous and the current (new) version.

There are considerations and problems with this approach. In our AWS circumstances, running on Lambda, we won't face downtime while the instance switches over, and a half-good PaaS solution won't create massive headaches either. However, we should primarily concern ourselves by considering application-level issues, rather than low-level technical issues. So when that "all at once" deployment is done, if something problematic surfaces that was not caught by testing, there is no obvious way how to proceed. Rollback? Roll forward? Hotfix, yay or nay? Sarcastic note: It's hardly unheard of that even manual testing and code freezes slip up.

Let's be honest: **Shit happens anyway**.

![Words to live by, as told by Mike Tyson](../img/tyson.jpg)

Using a [canary release](https://martinfowler.com/bliki/CanaryRelease.html) (formally in AWS CodeDeploy: "[blue/green](https://martinfowler.com/bliki/BlueGreenDeployment.html)") is one way to get those [unknown unknown effects](https://en.wikipedia.org/wiki/Cynefin_framework) happening with real production traffic in a safe and controlled manner. This is where [testing-in-production](https://increment.com/testing/i-test-in-production/) really kicks in—trying to answer questions no staging environment or typical test can address. Instead of being overly defensive, let's simply embrace the uncertainty, as it's already there anyway.

**🎯 Example**: Setting the line to `type: Canary10Percent5Minutes` makes the deployment happen through AWS CodeDeploy in a more controlled, bi-directed fashion: 90% of the traffic will pass to whatever function version that was already deployed and active, while the remaining 10% of traffic will be directed to the "canary" version of the function. The alarm configuration (defined on lines 75-83) looks for a static value of 3 or more errors on the function (I assume all versions here?) during the last 60-second window. After 5 minutes, given nothing has fired the alarm, then the new version takes all of the traffic.

You can either manually send "error traffic" with the `erroruser@company.com` Authorization header, or use the AWS CLI to manually toggle the alarm state. See [AWS docs for how to set the alarm state](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudwatch/set-alarm-state.html), similar to:

```bash
aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"
```

_Tip: Use the above with the `OK` state value to reset the alarm when done._

This specific solution is rudimentary, but indicative enough for how a canary solution might begin to look.

_See this article at_ [_Google Cloud Platform for more information on deployment and test strategies_](https://cloud.google.com/architecture/application-deployment-and-testing-strategies)_._
