---
description: >-
  We can go full monty and add better, richer observability tooling when we feel
  like we've outgrown the AWS standard tooling.
---

# 👀 Additional observability

[Honeycomb](https://www.honeycomb.io) is a next-generation observability-focused solution. Honeycomb or a similar solution could be bolted on as an addition if you want some more bells and whistles beyond what AWS provides. Many modern observability tools are based on the OpenTelemetry standard, making a choice basing itself on that standard a fairly future-proof decision.

{% hint style="info" %}
Read about [OpenTelemetry here](https://leaddev.com/monitoring-observability/rise-opentelemetry).
{% endhint %}

{% hint style="success" %}
If you want to toy with Honeycomb, look no further than [their playground](https://www.honeycomb.io/play/).
{% endhint %}

If you want to try Honeycomb with this project, it's pretty easy if we use their [Lambda extension](https://github.com/honeycombio/honeycomb-lambda-extension):

1. [Get a free (generous!) Honeycomb account](https://www.honeycomb.io).
2. When you have the account set up and are logged in, get an API key (it's enough that it can send events and create datasets). [See the docs for more details](https://docs.honeycomb.io/api/api-keys/).
3. Modify the provided logger as per below, [since the regular `console.log()` does not seem to work correctly with Honeycomb](https://docs.honeycomb.io/getting-data-in/integrations/aws/aws-lambda/#javascript).
4. Modify [`serverless.yml`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/serverless.yml), uncommenting and adding your values to the environment variables `LIBHONEY_DATASET` and `LIBHONEY_API_KEY` (lines 37-38). **Note**: Ideally these are encrypted or stored in a secrets manager, but for this project, let's just do it this way.
5. Deploy the stack.
6. Add the Honeycomb Lambda integration layer by running `sh honeycomb-layer.sh` (ensure the values correspond with your values first!).
7. Ready to go! You should see data coming into Honeycomb shortly if you start using your live endpoints.

## Setting up Bunyan as a logger

**You might want to use Bunyan rather than our custom logger if the logs don't quite show up structured the way we output them**.

Install [`bunyan`](https://github.com/trentm/node-bunyan) and its typings with `npm install bunyan @types/bunyan`.

Open up [`src/FakeUser/frameworks/Logger.ts`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/src/FakeUser/frameworks/Logger.ts) and add this to the top of the file:

{% code title="src/FakeUser/frameworks/Logger.ts" %}
```typescript
import bunyan from "bunyan";
const log = bunyan.createLogger({ name: "better-apis-workshop" });
```
{% endcode %}

For the `log()`, `warn()` and `error()` methods, change the existing `console.log()`-dependent implementation lines to:

* `log.info(createdLog);`
* `log.warn(createdLog);`
* `log.error(createdLog);`

Lastly, in the `createLog()` method, go ahead and remove the `level` field, as `bunyan` adds that itself.

Now you can use Bunyan instead of regular `console.log()` or our own custom one!
