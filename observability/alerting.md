# 🔔 Alerting

Alerts (or alarms; same thing) are usually connected to metrics. When the metric is triggered, the alert goes off.

**🎯 Example**: We set up an alarm in [`serverless.yml`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/serverless.yml) on lines 75-83. This particular alarm gets attached to the `deploymentSettings` object and was (as seen previously) part of how we ensure that an alarm can cancel a canary deployment.

{% code title="serverless.yml" %}

```yml
custom:
  alerts:
    dashboards: true

functions:
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
      [...]
      alarms:
        - FakeUserCanaryCheckAlarm
```

{% endcode %}

{% hint style="info" %}

You can extend this behavior to, for example, communicate the alert to an SNS topic which in turn can inform a pager system, Slack/Teams, or send an email to a relevant person. A better way of doing this would probably use a shared service that can also keep a stored log of all events, rather than just relaying the alert directly to a source.

{% endhint %}
