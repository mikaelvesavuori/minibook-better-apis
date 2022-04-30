# Better APIs workshop: Enhancing an API for quality, stability, and observability

## Table of contents

1. [What is this?](#what-is-this)
2. [Project resources](#project-resources)
3. [Prior art](#prior-art)
4. [How to follow along](#how-to-follow-along)
5. [Context](#context)
6. [Scenario](#scenario)
7. [Workshop assignment](#workshop-assignment)
8. [My solution pitch](#my-solution-pitch)
9. [The application](#the-application)
10. [Architecture diagrams](#architecture-diagrams)
11. [Technical components](#technical-components)
12. [API documentation](#api-documentation)
13. [Implementation patterns and principles](#implementation-patterns-and-principles)

**Quality**

- 📝 [Make your processes known](#-make-your-processes-known)
- 🧱 [SOLID principles](#-solid-principles)
- 🛁 [Clean architecture](#-clean-architecture)
- 🏗️ [Continuous Integration and Continuous Deployment](#%EF%B8%8F-continuous-integration-and-continuous-deployment)
- 🛠️ [Refactor continuously ("boy scout rule")](#%EF%B8%8F-refactor-continuously-boy-scout-rule)
- 🛣️ [Trunk-Based Development](#%EF%B8%8F-trunk-based-development)
- 🥼 [Test-Driven Development](#-test-driven-development)
- 🧰 [Baseline tooling and plugins](#-baseline-tooling-and-plugins)
- 📜 [Generate documentation](#-generate-documentation)
- 🏭 [Test automation in CI](#-test-automation-in-ci)
- 🧪 [Unit testing (and more)](#-unit-testing-and-more)
- 🤖 [Synthetic testing](#-synthetic-testing)
- 🔁 [Automated scans](#-automated-scans)
- 🧾 [Generate a software bill of materials](#-generate-a-software-bill-of-materials)
- ☑️ [Open source license compliance](#%EF%B8%8F-open-source-license-compliance)
- 📝 [Release versioned software and produce release notes](#-release-versioned-software-and-produce-release-notes)

**Stability**

- 🗂️ [API client version using headers](#%EF%B8%8F-api-client-version-using-headers)
- 📄 [API schema](#-api-schema)
- 👌 [API schema validation](#-api-schema-validation)
- 🧬 [Branch by abstraction](#-branch-by-abstraction)
- 🆕 [Beta functionality](#-beta-functionality)
- 🏁 [Feature toggles ("feature flags")](#-feature-toggles-feature-flags)
- 🎭 [Authorization and role-based access](#-authorization-and-role-based-access)
- 🗺️ [Lifecycle management and roadmap](#%EF%B8%8F-lifecycle-management-and-roadmap)
- 🦺 [Canary deployment](#-canary-deployment)

**Observability**

- 🪵 [Structured logger](#-structured-logger)
- 🕵️ [AWS baseline observability](#%EF%B8%8F-aws-baseline-observability)
- 🛎️ [Alerting](#%EF%B8%8F-alerting)
- 📇 [Service discoverability and metadata](#-service-discoverability-and-metadata)
- 👁️ [Additional observability](#%EF%B8%8F-additional-observability)

14. [Tips and references](#tips-and-references)

# What is this?

_This is a simple workshop combined with a fairly elaborate demonstration of how to approach improving an API to production-grade levels._

Writing and maintaining APIs can be hard. While the cloud, serverless, and the microservices revolution made it easier and more convenient to set an API skeleton up, age-old issues like software quality ([SOLID](https://stackoverflow.blog/2021/11/01/why-solid-principles-are-still-the-foundation-for-modern-software-architecture/), etc) and understanding the needs of the API consumers still persist.

Werner Vogels, legendary CTO of Amazon, [states his API rules like this](https://www.youtube.com/watch?app=desktop&v=8_Xs8Ik0h1w):

```
1. APIs are Forever
2. Never Break Backward Compatibility
3. Work Backwards from Customer Use Cases
4. Create APIs That are Self Describing and Have a Clear, Specific Purpose
5. Create APIs with Explicit and Well-Documented Failure Modes
6. Avoid Leaking Implementation Details at All Costs
```

In practice, how can we begin moving towards those ideals?

This repo presents an application ("start" and "finish" formats) and a made-up but real-ish scenario that, taken together, practically demonstrate a range of techniques or methods, patterns, implementations, as well as tools, that all help enhance quality, stability, and observability of applications:

**Quality** means our applications are well-built, functional, safe and secure, maintainable, and are built to high standards.

**Stability** means that our application can withstand external pressure and internal change, without failing at predictably providing its key business values.

**Observability** means that we can understand, from the outputs of our application, what it is doing and if it is behaving well. Or as [Charity Majors writes](https://twitter.com/mipsytipsy/status/1305398051842871297):

```
📉 Monitoring is for running and understanding other people's code (aka "your infrastructure")

📈 Observability is for running and understanding *your* code -- the code you write, change and ship every day; the code that solves your core business problems.
```

Of the three above concepts, _stability_ is the most misunderstood one, and it will be the biggest and most pronounced component here. It will be impossible to reach what Vogels is pointing to, without addressing the need for stability.

_**Caveat**: No single example can fully encompass all details involved in such a complex territory as this, but at least I will give it a try!_

---

# Project resources

Feel free to check out the [generated static website on Cloudflare Pages](https://better-apis-workshop.pages.dev) and the [API docs on Bump](https://bump.sh/doc/better-apis-workshop).

---

# Prior art

You might be interested in a previous piece of work I've done: [multicloud-serverless-canary](https://github.com/mikaelvesavuori/multicloud-serverless-canary), especially if you want to see more on the Azure and GCP side of CI and canaries.

There's also [microservices-testing-workshop](https://github.com/mikaelvesavuori/microservices-testing-workshop) for a similar "workshop + demo" approach, but focusing on testing an event-driven serverless application.

---

# How to follow along

You can...

- **Go on a guided tour**: Grab a coffee, just read and follow along with links and references to the work.

or

- **Do the workshop part**: Clone the repo, run `npm install` and `npm start`, then read about the patterns and try it out in your own self-paced way.

## Commands

The below commands are those I believe you will want to use. See `package.json` for more commands!

- `npm start`: Runs Serverless Framework in offline mode
- `npm test`: Tests code
- `npm run deploy`: Deploys code with Serverless Framework
- `npm run release`: Run [standard-version](https://github.com/conventional-changelog/standard-version#cutting-releases)
- `npm run build`: Package and build the code with Serverless Framework

## Prerequisites

_These only apply if you want to deploy code._

- Amazon Web Services (AWS) account with sufficient permissions so that you can deploy infrastructure. A naive but simple policy would be full rights for CloudWatch, Lambda, API Gateway, X-Ray, S3, and CodeDeploy.
- GitHub account to host your Git fork and for running CI with GitHub Action.
- Create a mock API payload on Mockachino.
- **Suggested**: For example a Cloudflare account for hosting your static documentation on Cloudflare Pages.
- **Optional**: Bump account to host your API description. You can remove the Bump section from `.github/workflows/main.yml` if you want to skip this.

All of the above services can be had for free!

### 1. Clone or fork the repo

Clone and fork the repo as you normally would.

### Optional: Get a Bump account and token

_If you don't want to use Bump, go ahead and remove the part at the end of `.github/workflows/main.yml`._

Go to the [Bump website](https://bump.sh) and create a free account and get your token (accessible under `CI deployment`, see the `Access token` field).

**Copy the token for later use.**

### 2. Mockachino feature toggles

We will use [Mockachino](https://www.mockachino.com) as a super-simple mock backend for our feature toggles. This way we can continuously change the values without having to redeploy a service or anything else.

It's really easy to set up: Go to the website and paste this payload into the `HTTP Response Body`:

```json
{
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

Change the path from the standard `users` to `toggles`. Click `Create`.

You will get a "space" in which you can administer and edit the mock API. You'll see a link in the format `https://www.mockachino.com/spaces/SOME_RANDOM_LINK_HERE`.

**Copy the endpoint, you'll use it shortly.**

### 3. First deployment to AWS using Serverless Framework

Install all dependencies with `npm install`, set up [husky](https://github.com/typicode/husky) pre-commits with `npm run prepare`, then make a first deployment from your machine with `npm run deploy`.

We do this so that the dynamic endpoints are known to us; we have a logical dependency to these when it comes to the test automation.

**Copy the endpoints to the functions.**

### 4. Update references

Next, update the environment value in `serverless.yml` (around lines 35-36) to reflect your Mockachino endpoint:

```
environment:
  TOGGLES_URL: https://www.mockachino.com/YOUR_RANDOM_STRING/toggles
```

Next, also update the following files to reflect your Mockachino endpoint:

- `jest.env.js` (line 2)
- `tests/mocks/handlers.ts` (line 11-12)

Continue by updating the following files to reflect your FakeUser endpoint on AWS:

- `api/schema.yml` (line 8)
- `tests/integration/index.ts` (line 6-7)
- `tests/load/k6.js` (line 6)

If you chose to use Bump:

- Add your document name in the CI script `.github/workflows/main.yml` on line 113 (`doc: YOUR_DOC_NAME`)
- Update the reference to the Bump docs in `PROJECT.md` on line 43.

### Optional: Continuous Integration (CI) on GitHub

If you connect this repository to GitHub you will be able to use GitHub Actions to run a sample CI script with all the tests, deployments, and stuff. The CI script acts as a template for how you can tie together all the build-time aspects in a simple way. It should be easily portable to whatever CI platform you might otherwise be running.

You'll need a few [secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) set beforehand if you are going to use it:

- `AWS_ACCESS_KEY_ID`: Your AWS access key ID for a deployment user
- `AWS_SECRET_ACCESS_KEY`: Your AWS secret access key for a deployment user
- `FAKE_USER_ENDPOINT`: Your AWS endpoint for the FakeUser service, in the format "https://RANDOM.execute-api.REGION.amazonaws.com/shared/fakeUser" (known after the first deployment)
- `MOCKACHINO_ENDPOINT`: Your Mockachino endpoint for feature toggles, in the format "https://www.mockachino.com/RANDOM/toggles"
- `BUMP_TOKEN`: Your token for [Bump](https://bump.sh) which will hold your API docs (just skip if you don't want to use it; also remove from the CI script in that case)

### Optional: Deploy documentation to the web

If you have this repo in GitHub you can also very easily connect it through Cloudflare Pages to deploy the documentation as a website generated by TypeDoc.

You need to set the build command to `npm run build:hosting`, then the build output directory to `typedoc-docs`.

See the [Cloudflare Pages documentation](https://developers.cloudflare.com/pages/platform/github-integration) for more information.

_You can certainly use something like [Netlify](https://github.com/marketplace/actions/netlify-deploy) if that's more up your alley._

### 5. Deploy the complete project

You can now deploy the project manually or through CI, now that all of the configuration is setup.

Great work!

---

# Context

You will be especially interested in this project if you have ever been involved in situations like the ones below, and want to have ideas for how to address them:

- You inherited something that is "impossible to work with or understand"
- Your team was unable to deliver new features because changes would mean breaking them for someone else
- You built something but don't know who your consumers are
- You document in Confluence or Sharepoint or something like that. If your document at all. You don't, since there is no allotted time for it. OK so maybe the API sometimes, but it's never up to date, really. To be honest, you tell people "ask me if you have questions, don't trust the docs".
- You looked up what [DORA metrics](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) are and laughed out the words "yeah not my company no sirree never"
- You say that you are "autonomous" but someone always keeps freezing deployments to the shared staging environment so the only actual autonomous thing is `localhost`
- You say that you "implemented" continuous delivery, but it's still too painful to integrate and release without a gatekeeper and crashing a hundred systems
- You have heard "we cannot have any confidence in our systems if we don't do real, manual scheduled all-hands-on-deck testing with frozen versions of all external systems" so many times you actually have started to believe that ludicrous statement
- You wonder if things would be better with more infra and more environments, but start having nightmares when you do some back-of-the-napkin math on how many you'd need, not to mention the burden of supporting them logically

There are a million more of those, but you get the point; It's a dark and strange place to be in.

![Giovanni Battista Piranesi - Imaginary Prisons (1745-1761)](/images/piranesi.jpg)

> _[Giovanni Battista Piranesi - Imaginary Prisons (1745-1761)](https://en.wikipedia.org/wiki/Imaginary_Prisons)_

While it's fun to throw around the old Piranesi drawing or [Happiness in Slavery](https://imvdb.com/video/nine-inch-nails/happiness-in-slavery) video as self-deprecating memes, I think it is Escher who brilliantly captures the "positive" side of this mess—That things become very, very strange when multiple realities and gravities are seen _together, at once_. Even if one "reality" is perfectly stable and sound, the work of architects is to support all the realities that need to work together.

![M.C. Escher - Relativity (1953)](/images/escher.jpg)

> _[M.C. Escher - Relativity (1953)](<https://en.wikipedia.org/wiki/Relativity_(M._C.\_Escher)>)_

So: We need to get back some of that control so the totality does not look more like chaos than whatever it is that we are attempting to do.

At the end of the day, our work is about supporting the shared goals (business, organization, or personal goals, if it's a pet project) and letting us stay safe, sound, and happy working professionals while doing so.

---

# Scenario

You are supporting your friends, who are making a new social online game, by providing an API service that churns out fake users. For their initial MVP, they asked you to create a service that just returns hardcoded feedback; an object like so, `{ "name": "Someguy Someguyson" }`. This was enough while they built their network code and first Non-Player Character engine.

The code is as simple as:

```TypeScript
import { APIGatewayProxyEvent, Context, APIGatewayProxyResult } from 'aws-lambda';

/**
 * @description The controller for our "fake user" service, in its basic or naive shape.
 */
export async function handler(
  event: APIGatewayProxyEvent,
  context: Context
): Promise<APIGatewayProxyResult> {
  try {
    return {
      statusCode: 200,
      body: JSON.stringify({
        name: 'Someguy Someguyson'
      })
    };
  } catch (error) {
    return {
      statusCode: 500,
      body: JSON.stringify(error)
    };
  }
}
```

_Refer to [src/FakeUserBasic/index.ts](/src/FakeUserBasic/index.ts) for the above code._

Now, things are starting to become more involved:

- They are ready for a bit more detailed user data, meaning more fields.
- They are also considering pivoting the game to use cats, instead of humans.
- Of course, it's also ideal if the team can be sensibly isolated from any work on new features by individual contributors in the open-source community, who will need to have the work thoroughly documented and coded to a level that marks the high ambitions and makes it easy to contribute.
- Oh, and it would be good with a dedicated beta feature set, too.

For the near future, the **API must be able to handle all these use-cases**. It would also be perfect if the API can have **stable interfaces (for new clients and old), and not use several endpoints**, as the development overhead is already big for the two weekend game programmers.

How can you solve this situation?

---

# Workshop assignment

_Given the scenario, how would you approach addressing the requirements?_

The workshop addresses typical skills needed of a technical lead or solution architect, prioritizing making considered architectural choices and being able to clearly communicate them.

This is possible to do two ways, either _with_ or _without_ code, as is needed based on the audience.

**⏳ Suggested timeframe (no code)**: 1 hour.

or

**⏳ Suggested timeframe (with code)**: 2 hours. You can start from `src/FakeUserBasic/`.

The output should be a diagram or other visual artifact, and if also coding, a set of code that demonstrates a full or partial solution.

At the end of the period, present your solution proposal (~5-10 minutes).

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

---

# The application

The demo application provides an API that returns a "fake user":

- The first version (`current`) of the API returns a hardcoded name.
- The second version (`beta`) includes a name and some other fields retrieved from an external API, plus a profile image (of a cat) from another external API.
- Finally, a new feature is developed on top of the second version, using a third external API. This feature is hidden under a feature toggle named `enableNewUserApi`.

More flows and details can be seen in the diagrams below.

The main software components are:

- **Microservice**: The `FakeUserBasic` service, if you want to do the workshop part and have some boilerplate code ready
- **Microservice**: The `FakeUser` service
- **Microservice**: The `FeatureToggles` service

Based on the user toggles, the service calls out to the following external APIs:

- **catApi** @ [https://thatcopy.pw/catapi/rest/](https://thatcopy.pw/catapi/rest/)
- **JSONPlaceholder** @ [https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)
- **RandomUser** @ [https://randomuser.me/api/](https://randomuser.me/api/)

The feature toggles are fetched, as has been stated previously, from Mockachino, where you will have to create an endpoint with the toggles payload.

---

# Architecture diagrams

## Solution diagram

![Architecture diagram](/diagrams/better-apis-diagram.png "Architecture diagram")

## Deployment diagram

![Deployment diagram](/diagrams/deployment.png "Deployment diagram")

## Microservices

### FakeUser

![FakeUser](/diagrams/arkit-fakeuser.svg "FakeUser")

### FakeUserBasic

![FakeUser](/diagrams/arkit-fakeuserbasic.svg "FakeUserBasic")

### FeatureToggles

![FeatureToggles](/diagrams/arkit-toggles.svg "FeatureToggles")

---

# Technical components

Services used are:

- [Amazon Web Services](https://aws.amazon.com)
- Optional: [GitHub](https://github.com) and [GitHub Actions](https://github.com/features/actions)
- Optional: [Bump](https://bump.sh)
- Optional: [Cloudflare Pages](https://pages.cloudflare.com)

The infrastructure components are:

- [AWS API Gateway](https://aws.amazon.com/api-gateway/), to route incoming requests, handle request validation and authorize the user
- [AWS Lambda](https://aws.amazon.com/lambda/), for running our serverless microservices
- [AWS S3](https://aws.amazon.com/s3/), for storing the functions

This project uses these primary libraries:

- [Serverless Framework](https://www.serverless.com) to handle packaging and deploying the microservices
- [TypeScript](https://www.typescriptlang.org) as the language
- [Jest](https://jestjs.io) to run unit tests
- [MSW](https://mswjs.io) to mock external APIs
- [TripleCheck CLI](https://github.com/mikaelvesavuori/triplecheck-cli) to run contract tests
- [TypeDoc](https://typedoc.org) to generate documentation from the source code
- [Arkit](https://typedoc.org) to generate architecture diagrams from the source code

---

# API documentation

These apply to the full "finished" state.

## Fake user

Get a fake user.

**Required headers**

**`Authorization`**: `erroruser@company.com` | `legacyuser@company.com` |  `standarduser@company.com` | `betauser@company.com` | `qauser@company.com` | `devuser@company.com` |`devnewfeatureuser@company.com`

**`X-Client-Version`**: `1` | `2`

```bash
GET {{API_URL_BASE}}/shared/fakeUser
```

## Feature toggles

Send the user name (email) of a user to get their feature toggles. An unknown (or missing etc.) user will return default toggles.

```json
POST {{API_URL_BASE}}/shared/featureToggles

{
  "userName": "devnewfeatureuser@company.com"
}
```

---

# Implementation patterns and principles

_Note that what we are dealing with here is a REST API which is different from a gRPC and/or GraphQL API, which may have other traditions for how to, for example, handle versions or deprecation decorators. However, the overall techniques and patterns should apply mostly similarly._

These are listed under the greater scopes of [Quality](#quality), [Stability](#stability), and [Observability](#observability).

---

# Tips and references

This list may contain some links from the main sections but is intended primarily as a set of additional readings.

## Books

- [Accelerate: The Science of Lean Software and DevOps: Building and Scaling High Performing Technology Organizations](https://www.amazon.com/Accelerate-Software-Performing-Technology-Organizations/dp/1942788339), by Nicole Forsgren, Jez Humble, Gene Kim
- [Building Microservices: Designing Fine-Grained Systems (2nd Edition)](https://www.amazon.com/Building-Microservices-Designing-Fine-Grained-Systems/dp/1492034029), by Sam Newman
- [Clean Agile: Back to Basics](https://www.amazon.com/Clean-Agile-Back-to-Basics/dp/B08X7FWCZM), by Robert Martin
- [Clean Architecture: A Craftsman's Guide to Software Structure and Design](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164), by Robert Martin
- [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), by Robert Martin
- [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612), by Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides
- [Domain-Driven Design: Tackling Complexity in the Heart of Software](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215) by Eric Evans
- [Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577), by Vaughn Vernon
- [Refactoring: Improving the Design of Existing Code (2nd Edition)](https://www.amazon.com/Refactoring-Improving-Existing-Addison-Wesley-Signature/dp/0134757599), by Martin Fowler

## Articles and online resources

- [Charity Majors: I test in prod](https://increment.com/testing/i-test-in-production/)
- [Cindy Sridharan: Testing in Production, the safe way](https://copyconstruct.medium.com/testing-in-production-the-safe-way-18ca102d0ef1)
- [Heidi Waterhouse: Testing in Production to Stay Safe and Sensible](https://launchdarkly.com/blog/testing-in-production-for-safety-and-sanity/)
- [Danilo Sato: Canary Release](https://martinfowler.com/bliki/CanaryRelease.html)
- [IBM: Continuous Testing](https://www.ibm.com/cloud/learn/continuous-testing)
- [DDD Resources](https://www.domainlanguage.com/ddd/)
- [Martin Fowler: Domain Driven Design](https://martinfowler.com/bliki/DomainDrivenDesign.html)
- [Art Gillespie: Deploy != Release (Part 2)](https://blog.turbinelabs.io/deploy-not-equal-release-part-two-acbfe402a91c)
- [FeatureFlags.io](https://featureflags.io)
- [Trunk Based Development](https://trunkbaseddevelopment.com)
- [Branch By Abstraction?](https://www.branchbyabstraction.com)
- [REST API Design - Resource Modeling](https://www.thoughtworks.com/insights/blog/rest-api-design-resource-modeling)
- [Clean architecture for the rest of us](https://pusher.com/tutorials/clean-architecture-introduction/)
- [Refactoring.guru](https://refactoring.guru)
- [Google Testing Blog](https://testing.googleblog.com)
- [Yubl’s road to Serverless — Part 2, Testing and CI/CD](https://medium.com/hackernoon/yubls-road-to-serverless-part-2-testing-and-ci-cd-72b2e583fe64)
- [Google Cloud Architecture Center: Application deployment and testing strategies](https://cloud.google.com/architecture/application-deployment-and-testing-strategies)
- [APIsecurity.io](https://apisecurity.io)
- [API Security Encyclopedia](https://apisecurity.io/encyclopedia/content/api-security-encyclopedia)
- [Why SOLID principles are still the foundation for modern software architecture](https://stackoverflow.blog/2021/11/01/why-solid-principles-are-still-the-foundation-for-modern-software-architecture/)
- [Khalil Stemmler: How to Test Code Coupled to APIs & Databases](https://khalilstemmler.com/articles/test-driven-development/how-to-test-code-coupled-to-apis-or-databases/); also available as video (see below)

## Video

- [AWS re:Invent 2021 - Keynote with Dr. Werner Vogels](https://www.youtube.com/watch?app=desktop&v=8_Xs8Ik0h1w)
- [Khalil Stemmler: How to Test Code Coupled to APIs & Databases](https://www.youtube.com/watch?v=ajfZqzeHp1E)
