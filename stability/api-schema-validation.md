---
description: >-
  Learn how to validate incoming requests before they enter the application
  layer.
---

# 👌 API schema validation

AWS API Gateway offers [request (schema) validation](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-method-request-validation.html). Schema validation is done with [JSON schemas](https://json-schema.org) which are similar to, but ultimately not the same as, OpenAPI schemas.

These validators allow our gateway to respond to incorrect requests without us needing to do much of anything about them, other than provide the validator. Also, we have the benefit of our Lambda functions not running if the ingoing input is not looking the way we expect it to.

{% hint style="success" %}
It can't be understated how important it is to actually use the capabilities of the cloud components/services we are using. It's _primitive and wrong_ to have to do **basic** request validation in our application layer—use the built-in capabilities in API Gateway and similar services and do more business-oriented validation as needed in the application instead.
{% endhint %}

Once again, since we have a manual approach, any validation schemas need to be handled separately from our code and the OpenAPI schema.

{% hint style="info" %}
We only use validators for POST requests, which means the `FeatureToggles` function is in scope, but not the `FakeUser` function.
{% endhint %}

**🎯 Example**: See [`api/FeatureToggles.validator.json`](https://github.com/mikaelvesavuori/better-apis-workshop/blob/main/api/FeatureToggles.validator.json). It's attached to our feature toggles function in `serverless.yml` on lines 96-98.

{% code title="serverless.yml" %}
```yml
FeatureToggles:
  handler: src/FeatureToggles/controllers/FeatureTogglesController.handler
  description: Feature toggles
  events:
    - http:
        method: POST
        path: /featureToggles
        request:
          schema:
            application/json: ${file(api/FeatureToggles.validator.json)}
```
{% endcode %}
