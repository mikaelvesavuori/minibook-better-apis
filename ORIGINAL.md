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

# Implementation patterns and principles

_Note that what we are dealing with here is a REST API which is different from a gRPC and/or GraphQL API, which may have other traditions for how to, for example, handle versions or deprecation decorators. However, the overall techniques and patterns should apply mostly similarly._

These are listed under the greater scopes of [Quality](#quality), [Stability](#stability), and [Observability](#observability).
