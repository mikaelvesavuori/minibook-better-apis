---
description: TODO
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
