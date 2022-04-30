### 🛣️ Trunk-Based Development

[Trunk Based Development](https://trunkbaseddevelopment.com) is worth reading about, if nothing else because it seems misunderstood by some.

**🎯 Example 1**: I can't easily point to some evidence here, but the full history of this project has been handled this way.

For me, TBD captures an essential truth: That most branching models are just too complicated, and have too many adverse effects when it comes to merging code and staying agile. Even if certain models (IMHO) balance those aspects fairly well, like [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) (not to be mixed up with [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/)!), nothing is simpler and more core to the agile values than TBD: Just push to `main` (or `master` if you aren't up with the times) and trust your tooling to handle it.

_Note that even Vincent Driessen, the original conceiver of GitFlow, nowadays actively discourages the use of GitFlow in modern circumstances._

The pros and cons of TBD are of course only fully visible when seen together with other practices and tools you have. There needs to be certain maturity before doing this while remaining safe. This project should represent perfectly valid conditions for which TBD can be used instead of less-agile strategies.

**🎯 Example 2**: To truly support TBD, I've added [husky](https://github.com/typicode/husky) to run pre-commit hooks for various activities, such as testing. This way we get to know even before the code is "sent off" if it's in shape to reach the CI stage.

Read more about Git workflow anti-patterns at [Jason Swett's blog](https://www.codewithjason.com/git-workflow-anti-patterns/).

_Tip: You can usually set up various branching strategies and restrictions in your CI tool, to effectively require TBD._

### 🥼 Test-Driven Development

[Test-driven development](https://testdriven.io/test-driven-development/) is a practice that a lot of people swear by. I'm a 50/50 person myself—sometimes it makes sense to me, sometimes I just write the tests after I feel I'm out of the weeds when it comes to the first implementation. No need to be a fundamentalist; be a pragmatist! The important thing is that _there are tests_, not so much how and when they came.

However, in case you want to be a good-spirited TDD crusader then I've made it easy to do so.

**🎯 Example**: Just run `npm run test:unit:watch` and Jest will watch your tests and source code. You can also modify what Jest "watches" in the interactive dialog.

### 🧰 Baseline tooling and plugins

This project uses two very common—but hugely effective—tools, namely [ESLint](https://eslint.org) and [Prettier](https://prettier.io). These two ensure that you have a baseline, pluggable way of ensuring similar standards (and automation of them) across a team. Really, the first thing you want to make sure of is that the code looks and reads the same, regardless of who wrote it. Using these, now you can.

_Don't forget to enable "fix on save"! Also consider the VS Code plugins for [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) and [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)._

When it comes to more IDE-centric plugins in the security department, I highly recommend the [Snyk Vulnerability Scanner](https://marketplace.visualstudio.com/items?itemName=snyk-security.snyk-vulnerability-scanner) (the successor to [vulncost](https://github.com/snyk/vulncost)) for [Visual Studio Code](https://code.visualstudio.com). Other nice ones include:

- [Abracadabra, refactor this!](https://marketplace.visualstudio.com/items?itemName=nicoespeon.abracadabra)
- [Checkov](https://marketplace.visualstudio.com/items?itemName=Bridgecrew.checkov)
- [DevSkim](https://marketplace.visualstudio.com/items?itemName=MS-CST-E.vscode-devskim)
- [OpenAPI (Swagger) Editor](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)

**🎯 Example**: You'll see that this project has configurations files such as `.eslintrc` and `.prettierrc` laying around. I'd not write many lines without those tools around.

### 📜 Generate documentation

While it would be unrealistic to say that you shouldn't need to write documentation, we can at least get to the point where documentation (given that some exist) can be generated and output through automation.

Here, we use the [JSDoc](https://jsdoc.app) standard for documenting source code.

**🎯 Example 1**: Open up any TS file in the `src` folders.

Then, we use [TypeDoc](https://typedoc.org) for generating docs from the source code.

**🎯 Example 2**: You can generate documentation into the `typedoc-docs` folder by running `npm run docs`. These are the documents uploaded to [the static website](https://better-apis-workshop.pages.dev) as well.

For diagrams, we use [Arkit](https://arkit.pro/) for generating diagrams from your software architecture.

**🎯 Example 3**: See the Arkit diagrams above. Just as above, generate them with `npm run docs`.

Taken together, this means that you can easily make rich documentation available in a central, visible location (like a static website in this case) in the CI stage. This is precisely what I've done on my end for this project: These diagrams also get uploaded to Cloudflare Pages on each commit to `main`.

**🎯 Example 4**: See `.github/workflows/main.yml` for the CI script.

### 🏭 Test automation in CI

First, let's look at what I call "defensive testing", that is, any typical testing that we do to test the resiliency and functionality of our solution.

We need to establish clear boundaries and expectations of where our work ends, and someone else's work begins. To not be overloaded with other's services (though we are dependent on them) we use API mocking to mock away external dependencies. For this purpose, we use [Mock Service Worker](https://mswjs.io).

**🎯 Example 1**: See `tests/mocks/index.ts`.

Next up, we set up contract testing, using [TripleCheck CLI](https://github.com/mikaelvesavuori/triplecheck-cli). Contract testing means that we can easily verify if our assumptions of services and their interfaces are correct.

_For wider scale and bigger system landscapes, consider using the [TripleCheck broker](https://github.com/mikaelvesavuori/triplecheck-broker) to be able to store and load contracts from a centralized source._

**🎯 Example 2**: See `triplecheck.config.json` and the GitHub documentation linked above.

During the CI we will deploy a complete, realistic stack with the most recent version. First, we'll do some basic smoke tests, to verify we don't have a major malfunction on our hands. _In reality, smoke tests are just lighter types of integration tests._

**🎯 Example 3**: See `tests/synthetics/smoketest.sh`; it's just a very basic `curl` call!

When it comes to _actual_ integration testing of the real service we will do it when we've seen our smoke tests pass. My solution is a home-built thingy that makes some calls and evaluates the expected responses with the received data using [ajv](https://ajv.js.org).

**🎯 Example 4**: See `tests/integration/index.ts`.

### 🧪 Unit testing (and more)

At the end of the day, testing is about building confidence in our code. We need to have enough to adequately say our code is covered, and that we feel confident about what we wrote.

**🎯 Example**: You'll see that the tests under `tests/` are segmented into various categories. The tests under `tests/unit` are for the individual, relevant layers such as controllers, entities, and frameworks.

If you are writing tests _after_ having created the initial code—functions, classes, etc—then a good (and fast!) way to create baseline coverage and confidence is by writing tests for the high-level layers, such as the controllers. Some people call these fuller, outer-boundary tests "component tests" since they address full use cases. Testing this way, you have the immediate benefit of understanding your code as an API, as this is the closest to (or even the exact same) code that will be "the real deal" in production. These tests, therefore, tend to be very similar when writing for the integration and contract test use-cases: after all, they all try to address the same (or similar) things through more or less the exact same API.

Such coarse-grained API/integration/contract/component/unit tests also typically exercise a wider portion of your codebase, though normally not all of the error handling and such. _This is when you want to start testing on a finer level and not only at the very edge of your code._

For "frameworks" (such as utilities and idempotent calculations) these should be written so that they are very easy to test in isolation. These tend to be the easiest and least messy parts to test. Test both "success" and "failure" states as far as logically possible and meaningful.

While there is no such thing as "untestable" code, in practice if something is hard as nails to test, then it's probably the code (not the test) that needs refactoring.

If you later discover unknown test cases that need to be added, to guard against those, we just add them to the unit test collection. Some people call these "regression tests" but it's all the same in the end.

_Spending even a bit of time raising the overall coverage to 90% or more will provide valuable confidence to you and your team. Remember that there are diminishing returns after a certain point, and you should feel comfortable about just leaving some things untested, and especially so if the uncovered areas are not able to create meaningful problems._

### 🤖 Synthetic testing

Synthetic testing sounds cool and intriguing but is really no more than a directed stream of traffic from a non-human source, such as a computer.

Above we used a smoke test, which is absolutely a synthetic test, but there is another dimension I want to bring up as well: We can use synthetic testing to continuously verify that our solution keeps performing as expected. In that regard, we can think of it as a kind of "offensive testing" mechanism.

Running synthetic traffic is something that can certainly stress the existing instance, but in our case we could also leverage it during the deployment window (when rolling out a canary deployment) to more fully exercise the system, flushing out any issues. This would also give us a higher probability of hitting any issues—which we may not in a, for example, 10-minute window with low organic traffic.

**🎯 Example**: We can use load testing to run various larger-scale synthetic traffic volumes as one-offs to stress the API. Under `tests/load/k6.js` you can see our [k6](https://k6.io) script that we run in CI. The use-case we have here is to ensure that it functions correctly with a range of inputs rather than just a quick poke-and-feel as with the smoke test.

Synthetics can be run with almost anything: Examples include a regular API client like [Insomnia](https://insomnia.rest), a load testing tool like [k6](https://k6.io) or [Artillery](https://www.artillery.io), a SaaS like [Checkly](https://www.checklyhq.com), or a managed tool like [AWS CloudWatch Synthetics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html). Even `curl` works fine! If you want to set something up to continuously run against your endpoint, I recommend using an easy-to-use tool like CloudWatch Canaries or Checkly, directing them to your endpoint.

### 🔁 Automated scans

A best practice is to use various types of scans to automate often boring, sometimes hard, sometimes also mandated requirements e.g. for compliance and security aspects.

**🎯 Example 1**: We use [Trivy](https://github.com/aquasecurity/trivy) to check for vulnerabilities in packages (among other places).

**🎯 Example 2**: Then, we use [Checkov](https://www.checkov.io) to scan for misconfigurations, and also create an infrastructure-as-code SBOM (["software bill-of-materials"](https://en.wikipedia.org/wiki/Software_bill_of_materials)).

**🎯 Example 3**: Finally, we use [gitleaks](https://github.com/zricethezav/gitleaks) for detecting hard-coded secrets in our repository.

### 🧾 Generate a software bill of materials

We need to understand what our software is composed of—this is called "software composition analysis" (SCA).

**🎯 Example**: We create an SBOM just like above but this time for our packages, using [Syft](https://github.com/anchore/syft).

### ☑️ Open source license compliance

We must only use good, well-establish open source licenses.

**🎯 Example**: We verify that we only use a set of allowed open-source licenses, using [license-compliance](https://www.npmjs.com/package/license-compliance), and can also use [license-compatibility-checker](https://www.npmjs.com/package/license-compatibility-checker) to check for compatibility between our license and used ones'. You can see the license set in `package.json` under the `licenses` script, as being `MIT;ISC;0BSD;BSD-2-Clause;BSD-3-Clause;Apache-2.0;Unlicense;CC0-1.0`.

### 📝 Release versioned software and produce release notes

Each release should be uniquely versioned. We should also keep release notes, but it's one of those things that can be easy to miss.

**🎯 Example**: Instead of writing manual release notes, we use [Standard Version](https://github.com/conventional-changelog/standard-version) to output them for us from our commits into the `CHANGELOG.md` file. Practically, this works by running `npm run release`.

## Stability

### 🗂️ API client version using headers

The first way to define expectations is to use API versioning. While there are several ways to do this—for example, refer to [this article from Nordic APIs](https://nordicapis.com/everything-you-need-to-know-about-api-versioning/)—we are going to use a header to define which API version to use.

**🎯 Example**: See `src/FakeUser/controllers/FakeUserController.ts` and note how the header is handled in `checkInput()`.

Note: An alternative, perhaps more commonly used, approach would be to deploy a new instance of the API (like `api.com/v2`), but of course this would create further hardware segregation, which we want to avoid.

So, while it may be non-standard, in this context version 2 of the API represents the beta, meaning that version 1 represents the current (or "stable", "old") variant.

With this, we have created a way to define output simply through a header, without resorting to separate codebases or separate deployments.

_[GraphQL is a different beast when it comes to versioning](https://graphql.org/learn/best-practices/#versioning). See this section as REST-specific in its details, but a good idea overall as long as you adapt to what versioning means for your protocol._

### 📄 API schema

An API schema describes our API in a standardized way. You can write schemas by hand, use tooling to generate them, or go with services like [Stoplight](https://stoplight.io), [Bump](https://bump.sh), [Readme](https://readme.com), and API clients like [Insomnia](https://insomnia.rest/product/design) that sometimes have capabilities to design APIs, too.

**When you actually have a schema, make sure to make it accessible and visible (that's our reason for using Bump in this demo).**

There are a few ways to think about schemas, like ["API design-first"](https://www.infoq.com/articles/api-mocking-break-dependencies/), in which we design the API and generate the actual code from the schema. Our way is more traditional since we create the code and keep the schema mostly as a representation (however: a very important representation!).

We use the [OpenAPI 3](https://swagger.io/specification/) standard. The approach is manual, and hardly the most forward-facing option, but it's easy to understand and lets us (for what it's worth) implement the API as we need while trying to stay true to the schema specification. Learn more about it over at [Swagger](https://swagger.io/docs/specification/basic-structure/) and [Documenting APIs](https://idratherbewriting.com/learnapidoc/docapis_introtoapis.html).

**🎯 Example**: See `api/schema.yml` for the OpenAPI 3 schema. Since our approach is manual, we have to implement any security and/or validations on our end, in code. In our case, this is both for ingoing and outgoing data. Ingoing data can be seen handled at `src/FakeUser/controllers/FakeUserController.ts` in `checkInput()`, and outgoing data is handled in `src/FakeUser/entities/User.ts` and its various validation functions like `validateName()`.

_Strongly consider using security tooling like 42Crunch's [VS Code plugin for OpenAPI](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)._

Note that because this is intended as a public API, the OAS security object is not present.

_For GraphQL, consider if something like [Apollo Studio](https://www.apollographql.com/docs/studio/) might be a way to cover this area for your needs._

### 👌 API schema validation

AWS API Gateway offers schema validation. Schema validation is done with [JSON schemas](https://json-schema.org), which are similar but ultimately not the same as OpenAPI schemas.

These validators allow our gateway to respond to incorrect requests without us needing to do much of anything about them, other than provide the validator. Also, we have the benefit of our Lambda functions not running if the ingoing input is not looking the way we expect it to.

Once again, since we have a manual approach, any validation schemas need to be handled separately from code and the OpenAPI schema.

We only use validators for POST requests, which means the FeatureToggles function is in scope, but not the FakeUser function.

**🎯 Example**: See `api/FeatureToggles.validator.json`. It's attached to our feature toggles function in `serverless.yml` on lines 96-98.

### 🧬 Branch by abstraction

In Paul Hammant's words:

> [Branch] by abstraction instead of by [code] branching in source control. And no, that doesn't mean sprinkle conditionals into your source code, it means to use an abstraction concept that's idiomatic for the programming language you are using.

It's a methodical, very practical thing too:

```
1. Introduce an abstraction to methodically chomp away at that time consuming non-functional change
2. Start with a single most depended on and least depending component
3. To not jeopardize anyone else's throughput, work in a place in the codebase that is separate from the existing code
4. Methodically complete the work, temporarily duplicating tens or hundreds of components
5. Go live in a 'dark deployment' mode part complete as many times as needed
6. Scale up your CI infrastructure to guard old and new implementations
7. When ready, switch over in production (a toggle flip to end 'dark deployment' mode)
8. Lastly, delete old implementations and the abstraction itself (essential follow up work)
```

**🎯 Example**: While our example might be too lightweight, we do have a "beta" version and a "current" version as two code paths (see `src/FakeUser/controllers/FakeUserController.ts`, lines 37-44), abstracted via the controller. Effectively, we are—even in this contrived example—making it easy and possible to work together with both new and old code without too much risk. Taking this thinking further, it's possible to solve vastly more complex situations just as well.

Refer to the [Branch by Abstraction website](https://www.branchbyabstraction.com).

### 🆕 Beta functionality

We have built-in a "beta functionality" concept that is propagated from toggles into our services. This is a catch-all for new features that we want to test, and which may not yet be ready for wider release. This means that our services need to have distinct checks for this.

As you can see further below in the feature toggles section, we can also use user groups to segment features, as well as classic, individual toggles that can be used on a per-user basis.

What gives? Aren't these beta features just like other toggles? **Yes**. In this project, we define ”beta features” as a _feature-level, wide bucket across user groups_, while user grouping is a dynamically wide bucket (basically just audience segmentation). This way, we can define granularly both on user groups and on beta usage.

**🎯 Example**: See for example `src/FakeUser/usecases/createFakeUser.ts` that uses the provided toggles when calling the User entity which dynamically creates different types of users from this toggle.

### 🏁 Feature toggles ("feature flags")

This demo uses a handcrafted, simple feature flags engine and a small toggle configuration.

While a technically trivial solution, this enables a dynamic configuration that can be detached from the source code itself. [In effect, this means we are one big step closer to separating a (technical) deployment from a (business-oriented) release.](https://medium.com/turbine-labs/deploy-not-equal-release-part-two-acbfe402a91c)

The feature function is located at `src/FeatureToggles`. For the configuration part, we should put it on [Mockachino](https://www.mockachino.com) so that we can approximate a more conventional feature toggles service, getting (and updating) flags without having to re-deploy our code. The set of feature toggles is ordered under each respective user group.

Unknown (or missing, non-existing, null...) users get the `standard` user group access.

**🎯 Example**: The full configuration you can use as your template looks like the one below. Notice how it's segmented on the group level:

```json
{
  "error": {
    "enableBetaFeatures": false,
    "userGroup": "error"
  },
  "legacy": {
    "enableBetaFeatures": false,
    "userGroup": "legacy"
  },
  "beta": {
    "enableBetaFeatures": true,
    "userGroup": "beta"
  },
  "standard": {
    "enableBetaFeatures": false,
    "userGroup": "standard"
  },
  "dev": {
    "enableBetaFeatures": true,
    "userGroup": "dev"
  },
  "devNewFeature": {
    "enableBetaFeatures": true,
    "enableNewUserApi": true,
    "userGroup": "devNewFeature"
  },
  "qa": {
    "enableBetaFeatures": false,
    "userGroup": "qa"
  }
}
```

You can update the flags as you want, to get the effects you need, without requiring any redeployment!

_Note that different feature toggle services or implementations may differ in how they exactly apply toggles, and how they work. In our case here, we apply group-level toggles rather than request individual flags, if nothing else than for simplicity of demonstration._

Refer to [FeatureFlags.io](https://featureflags.io) and to [Unleash](https://www.getunleash.io)'s documentation on [activation strategies](https://docs.getunleash.io/user_guide/activation_strategy) for inspiration.

### 🎭 Authorization and role-based access

You'll often see tutorials and such talking about [authentication](https://auth0.com/intro-to-iam/what-is-authentication/), which is about how we can verify that a person really is that person.

[Authorization](https://www.osohq.com/academy), on the other hand, is knowing what this person is allowed to do.

_Authentication is entirely out of scope here, whereas trivial authorization IS in scope._

**🎯 Example 1**: In `serverless.yml` (lines 60-62) we define an authorizer function, located at `src/FeatureToggles/controllers/AuthController.ts`. Then on lines 70-74, we attach it to the `FakeUser` function. Now, the authorizer will run before the FakeUser function when called.

In our self-contained, basic demo we use a handcrafted [RBAC](https://en.wikipedia.org/wiki/Role-based_access_control) (role-based access control) to attach a user group to each of the users. The user group is added as a flag in the subsequent call to the actual service.

**🎯 Example 2**: In `src/FeatureToggles/config/userPermissions.ts` users are matched like so:

```TypeScript
export const userPermissions: Record<string, UserGroup> = {
  'erroruser@company.com': 'error',
  'legacyuser@company.com': 'legacy',
  'betauser@company.com': 'beta',
  'standarduser@company.com': 'standard',
  'devuser@company.com': 'dev',
  'devnewfeatureuser@company.com': 'devNewFeature',
  'qauser@company.com': 'qa'
};
```

For more full-fledged implementations, consider tools like [Oso](https://www.osohq.com) to authorize on the overall "access level". [See for example this demo](https://www.osohq.com/post/add-authorization-to-a-serverless-nodejs-app) using a similar serverless stack, or the plain [quick start version](https://docs.osohq.com/node/getting-started/quickstart.html).

### 🗺️ Lifecycle management and roadmap

We can follow the convention where we divide an API's lifecycle into design, lifetime, sunset, and deprecation phases (see resources below). Beyond these, we add the notion of being "removed" which is the point at which a feature has been completely purged from the source code.

**🎯 Example**: We'd keep a roadmap like the below in our docs. Imagine that today is 25 November 2021 and see below what our code base represents:

| Feature                                   | Group/Flag            | Beta start      | Stable          | Sunset         | Deprecated       | Removed         |
| ----------------------------------------- | --------------------- | --------------- | --------------- | -------------- | ---------------- | --------------- |
| Use hardcoded response                    | N/A                   | 1 August 2021   | 8 August 2021   | 1 October 2021 | 15 November 2021 | 15 January 2022 |
| Use RandomUser instead of JSONPlaceholder | Group `devNewFeature` | 1 November 2021 | 15 January 2022 | N/A            | N/A              | N/A             |

Refer to the pattern [Aggressive Obsolescence](https://microservice-api-patterns.org/patterns/evolution/AggressiveObsolescence.html) and articles by [Nordic APIs](https://nordicapis.com/how-to-smartly-sunset-and-deprecate-apis/) and [Stoplight](https://blog.stoplight.io/deprecating-api-endpoints).

### 🦺 Canary deployment

The traditional way to deploy software is as one big block that becomes instantly activated whenever it's deployed to a machine. If the code was a complete bomb, then you have zero time to verify this. This notion is what makes managers ask for counter-intuitive things like code freeze and all-hands-on-deck deployments. Let's forever end those days!

In `serverless.yml` at line ~85, you'll see `type: AllAtOnce`. This means that we get a classic "deploy === release" pattern. When the deployment is done, the function version is active, with a clear cut-off between the previous and the current (new) version.

There are considerations and problems with this approach. In our AWS circumstances, running on Lambda, we won't face downtime while the instance switches over, and a half-good PaaS solution won't create massive headaches either. However, we should primarily concern ourselves by considering application-level issues, rather than low-level technical issues. So when that "all at once" deployment is done, if something problematic surfaces that was not caught by testing, there is no obvious way how to proceed. Rollback? Roll forward? Hotfix, yay or nay? Sarcastic note: It's hardly unheard of that even manual testing and code freezes slip up.

Let's be honest: **Shit happens anyway**.

![Words to live by, as told by Mike Tyson](/images/tyson.jpg)

Using a [canary release](https://martinfowler.com/bliki/CanaryRelease.html) (formally in AWS CodeDeploy: "[blue/green](https://martinfowler.com/bliki/BlueGreenDeployment.html)") is one way to get those [unknown unknown effects](https://en.wikipedia.org/wiki/Cynefin_framework) happening with real production traffic in a safe and controlled manner. This is where [testing-in-production](https://increment.com/testing/i-test-in-production/) really kicks in—trying to answer questions no staging environment or typical test can address. Instead of being overly defensive, let's simply embrace the uncertainty, as it's already there anyway.

**🎯 Example**: Setting the line to `type: Canary10Percent5Minutes` makes the deployment happen through AWS CodeDeploy in a more controlled, bi-directed fashion: 90% of the traffic will pass to whatever function version that was already deployed and active, while the remaining 10% of traffic will be directed to the "canary" version of the function. The alarm configuration (defined on lines 75-83) looks for a static value of 3 or more errors on the function (I assume all versions here?) during the last 60-second window. After 5 minutes, given nothing has fired the alarm, then the new version takes all of the traffic.

You can either manually send "error traffic" with the `erroruser@company.com` Authorization header, or use the AWS CLI to manually toggle the alarm state. See [AWS docs for how to set the alarm state](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudwatch/set-alarm-state.html), similar to:

```bash
aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM --state-reason "testing purposes"
```

_Tip: Use the above with the `OK` state value to reset the alarm when done._

This specific solution is rudimentary, but indicative enough for how a canary solution might begin to look.

_See this article at [Google Cloud Platform for more information on deployment and test strategies](https://cloud.google.com/architecture/application-deployment-and-testing-strategies)._

## Observability

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
