### 🪵 Structured logger

Another best practice is to treat logs as a source of data rather than as individual strings. To do so, we need to have a structured approach to outputting them. In this project, we'll use a handcrafted logging utility to help us do this in an easy way that nobody can fail using correctly.

I've provided a basic one that also uses `getUserMetadata()` to get metadata (correlation ID and user ID) that has been set in the environment at an early stage in the controller.

**🎯 Example**: See `src/FakeUser/frameworks/Logger.ts`. Implementation is as simple as importing it and then:

```TypeScript
const logger = new Logger();
logger.log('My message!');
logger.warn('My warning!');
logger.error('My error!');
```

_Note: As opposed to some solutions, in our case, the Logger will not replace the vanilla `console.log` (etc) so you will need to import it everywhere you want to use it._

Using it, your logs will then all follow the format:

```
{
  message: "My message!",
  level: "INFO" | "WARN" | "ERROR" <based on the call, i.e. logger.warn() etc.>,
  timestamp: <timestamp>,
  userId: <userId from metadata>,
  correlationId: <correlationId from metadata>
};
```

Good folks like [Yan Cui have written and presented on this matter many times](https://www.slideshare.net/Codemotion/yan-cui-how-to-build-observability-into-a-serverless-application-codemotion-amsterdam-2019) and you can certainly also opt-in to turnkey solutions like [`dazn-lambda-powertools`](https://github.com/getndazn/dazn-lambda-powertools).

### 🕵️ AWS baseline observability

I'm not going to go very far down the observability vs monitoring dialog here. Suffice to say that classic monitoring consists of [logs, metrics, and traces](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/ch04.html). This is pretty easy to set up, and we should aim at least at having these under control.

**🎯 Example**: Our `serverless.yml` configuration enables [X-Ray](https://aws.amazon.com/xray/) in the `tracing` object. By default, [CloudWatch](https://aws.amazon.com/cloudwatch/) log groups are created. Next, we add the [`serverless-plugin-aws-alerts`](https://github.com/ACloudGuru/serverless-plugin-aws-alerts) plugin to give us some basic metrics (throttles, etc.). [Read more here](https://www.serverless.com/blog/serverless-ops-metrics). Taken together, these give us a very good level of baseline observability right out of the box.

_Refer to the [CloudWatch Logs Insights query syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html) if you want to set up some nice log queries. Also note that a shortcoming with CloudWatch is that they don't seem ideal for cross-service logs, but they are just fine when it comes to checking on a specific service. This is the intention behind the coming Honeycomb addition below._

_Do read [Lightstep's good introductory article on tracing](https://lightstep.com/distributed-tracing/) if you are interested._

### 🛎️ Alerting

Alerts (or alarms; same thing) are usually connected to metrics. When the metric is triggered, the alert goes off.

**🎯 Example**: We set up an alarm in `serverless.yml` on lines 75-83. This particular alarm gets attached to the `deploymentSettings` object and was (as seen previously) part of how we ensure that an alarm can cancel a canary deployment.

_Tip: You can extend this behavior to, for example, communicate the alert to an SNS topic which in turn can inform a pager system, Slack/Teams, or send an email to a relevant person. A better way of doing this would probably use a shared service that can also keep a stored log of all events, rather than just relaying the alert directly to a source._

### 📇 Service discoverability and metadata

[Service discovery](https://stackoverflow.com/questions/37148836/what-is-service-discovery-and-why-do-you-need-it) seems to be a very hot topic among the Kubernetes crowd. With serverless FaaS like we are using here (AWS Lambda), that's not really an interesting discussion. However, discoverability is not just a technical question—it's also something that absolutely relates to the social, organic, and management layers.

At some point that one function will be one service, which will be hundreds of services and thousands of functions. How to keep track of them? To some extent, it's possible to get a high-level picture inside of AWS or by being a CLI crusader. Unfortunately, if you are not, then there is no option unless you buy, build, or adapt some open-source solution.

One such open-source solution is Spotify's [Backstage](https://backstage.io) which offers a broad set of ideas—no wonder since it's labeled as "an open platform for building developer portals". It's a bit heavy, but some pretty big players are starting to use it.

For a lighter-weight system, I'd think the reasonable data to pull in would be some of the metadata and processes and output we have generated and written previously. In a corporate context, we'd want to ensure the source of truth of the state of our systems/applications resides with the codebase (not in, say, Sharepoint or Confluence or similar), but that a fresh representation is always available in some less developer-oriented context, say on an internally accessible website.

**🎯 Example**: See `manifest.json` for a napkin sketch of how one could work with service metadata. Of course, this is not a functional solution, as you'd need something to extract that data and store it somewhere meaningful.

### 👁️ Additional observability

We can go full monty and add better, richer observability tooling when we feel like we've outgrown the AWS standard tooling.

[Honeycomb](https://www.honeycomb.io) is a next-generation "true" observability solution. This can be bolted on as an addition if you want some more bells and whistles over the AWS stuff. It's also based on the OpenTelemetry standard, making it pretty future-proof. [Read about OpenTelemetry here](https://leaddev.com/monitoring-observability/rise-opentelemetry).

If you want to toy with Honeycomb, look no further than [their playground](https://www.honeycomb.io/play/).

If you want to try Honeycomb with this project, it's pretty easy if we use their [Lambda extension](https://github.com/honeycombio/honeycomb-lambda-extension):

1. [Get a free (generous!) Honeycomb account](https://www.honeycomb.io).
2. When you have the account set up and are logged in, get an API key (it's enough that it can send events and create datasets). [See the docs for more details](https://docs.honeycomb.io/api/api-keys/).
3. Modify the provided logger as per below, [since the regular `console.log()` does not seem to work correctly with Honeycomb](https://docs.honeycomb.io/getting-data-in/integrations/aws/aws-lambda/#javascript).
4. Modify `serverless.yml`, uncommenting and adding your values to the environment variables `LIBHONEY_DATASET` and `LIBHONEY_API_KEY` (lines 37-38). Note: Ideally these are encrypted or in a secrets manager, but for this project, let's just do it this way.
5. Deploy the stack.
6. Add the Honeycomb Lambda integration layer by running `sh honeycomb-layer.sh` (ensure the values correspond with your values first!).
7. Ready to go! You should see data coming into Honeycomb shortly if you start using your live endpoints.

#### Setting up Bunyan as a logger

Install [`bunyan`](https://github.com/trentm/node-bunyan) and its typings with `npm install bunyan @types/bunyan`.

Open up `src/FakeUser/frameworks/Logger.ts` and add this to the top of the file:

```
import bunyan from 'bunyan';
const log = bunyan.createLogger({ name: 'better-apis-workshop' });
```

For the `log()`, `warn()` and `error()` methods, change the existing `console.log()`-dependent implementation lines to: `log.info(createdLog);`, `log.warn(createdLog);` and `log.error(createdLog);`, respectively.

In the `createLog()` method, go ahead and remove the `level` field, as `bunyan` adds that itself.
