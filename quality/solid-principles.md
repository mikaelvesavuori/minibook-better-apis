# SOLID principles

The very first (technical) thing is to respect that good code, despite programming language, is good code even over time. Follow wise conventions like SOLID to guide your daily work. See for example this [Stack Overflow article](https://stackoverflow.blog/2021/11/01/why-solid-principles-are-still-the-foundation-for-modern-software-architecture/) for a concise introduction.

**🎯 Example**: One example could be the use of "dependency inversion" when calling the `betaVersion()` function in `src/FakeUser/controllers/FakeUserController.ts`, as we send in the toggles for it to use. The "single responsibility principle" should hopefully also be evident throughout most of the code.

```TypeScript
/**
 * @description Handle the new (v2) beta version.
 */
async function betaVersion(toggles: Record<string, unknown>): Promise<APIGatewayProxyResult> {
  const response = await createFakeUser(toggles); // Run use case
  return {
    statusCode: 200,
    body: JSON.stringify(response)
  };
}
```
