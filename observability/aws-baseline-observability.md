# 🕵️ AWS baseline observability

I'm not going to go very far down the observability vs monitoring dialog here. Suffice to say that classic monitoring consists of [logs, metrics, and traces](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/ch04.html). This is pretty easy to set up, and we should aim at least at having these under control.

**🎯 Example**: Our `serverless.yml` configuration enables [X-Ray](https://aws.amazon.com/xray/) in the `tracing` object. By default, [CloudWatch](https://aws.amazon.com/cloudwatch/) log groups are created. Next, we add the [`serverless-plugin-aws-alerts`](https://github.com/ACloudGuru/serverless-plugin-aws-alerts) plugin to give us some basic metrics (throttles, etc.). [Read more here](https://www.serverless.com/blog/serverless-ops-metrics). Taken together, these give us a very good level of baseline observability right out of the box.

_Refer to the [CloudWatch Logs Insights query syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html) if you want to set up some nice log queries. Also note that a shortcoming with CloudWatch is that they don't seem ideal for cross-service logs, but they are just fine when it comes to checking on a specific service. This is the intention behind the coming Honeycomb addition below._

_Do read [Lightstep's good introductory article on tracing](https://lightstep.com/distributed-tracing/) if you are interested._
