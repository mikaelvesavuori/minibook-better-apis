# API schema validation

AWS API Gateway offers schema validation. Schema validation is done with [JSON schemas](https://json-schema.org), which are similar but ultimately not the same as OpenAPI schemas.

These validators allow our gateway to respond to incorrect requests without us needing to do much of anything about them, other than provide the validator. Also, we have the benefit of our Lambda functions not running if the ingoing input is not looking the way we expect it to.

Once again, since we have a manual approach, any validation schemas need to be handled separately from code and the OpenAPI schema.

We only use validators for POST requests, which means the FeatureToggles function is in scope, but not the FakeUser function.

**🎯 Example**: See `api/FeatureToggles.validator.json`. It's attached to our feature toggles function in `serverless.yml` on lines 96-98.
