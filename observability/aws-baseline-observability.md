---
description: What do we get from the box with just a little configuration?
---

# 🕵 AWS baseline observability

There's a lot written and discussed when it comes to _observability_ vs _monitoring_. Let's go with Google's definition taken from [DORA](https://cloud.google.com/devops):

> Monitoring is tooling or a technical solution that allows teams to watch and understand the state of their systems. Monitoring is based on gathering predefined sets of metrics or logs.
>
> Observability is tooling or a technical solution that allows teams to actively debug their system. Observability is based on exploring properties and patterns not defined in advance.

— Source: [Google: DevOps measurement: Monitoring and observability](https://cloud.google.com/architecture/devops/devops-measurement-monitoring-and-observability)

Further, let's also add that monitoring classically is said to consist of the three "pillars" [logs, metrics, and traces](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/ch04.html). In the cloud, these are all typically pretty easy to set up and we should aim to at least have these under control.

{% hint style="info" %}
Of the three pillars, tracing is maybe the least well-understood. Do read [Lightstep's good introductory article on tracing](https://lightstep.com/distributed-tracing/) if you are interested in that area!
{% endhint %}

**🎯 Example**: Our [`serverless.yml`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/serverless.yml) configuration enables [X-Ray](https://aws.amazon.com/xray/) in the `tracing` object. By default, [CloudWatch](https://aws.amazon.com/cloudwatch/) log groups are created. Next, we add the [`serverless-plugin-aws-alerts`](https://github.com/ACloudGuru/serverless-plugin-aws-alerts) plugin to give us some basic metrics (throttles, etc.).

{% hint style="info" %}
Read more about [metrics with Serverless Framework here](https://www.serverless.com/blog/serverless-ops-metrics).
{% endhint %}

Taken together, these give us a very good level of baseline observability right out of the box.

{% code title="serverless.yml" %}
```yml
provider:
  [...]
  tracing:
    apiGateway: true
    lambda: true

plugins:
  - serverless-plugin-aws-alerts

custom:
  alerts:
    dashboards: true
```
{% endcode %}

{% hint style="info" %}
Refer to the [CloudWatch Logs Insights query syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL\_QuerySyntax.html) if you want to set up some nice log queries. Also note that a shortcoming with CloudWatch is that they don't seem ideal for cross-service logs, but they are just fine when it comes to checking on a specific service. This is the intention behind the coming Honeycomb addition below.
{% endhint %}

You can now see dashboards (metrics) in both the Lambda function view and CloudWatch (plus the logs themselves) and get the full X-Ray tracing. It's just as easy as that! Done deal.
