# 🛣 Trunk-Based Development

[Trunk Based Development](https://trunkbaseddevelopment.com) is worth reading about, if nothing else because it seems misunderstood by some.

**🎯 Example 1**: I can't easily point to some evidence here, but the full history of this project has been handled this way.

For me, TBD captures an essential truth: That most branching models are just too complicated, and have too many adverse effects when it comes to merging code and staying agile. Even if certain models (IMHO) balance those aspects fairly well, like [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow) (not to be mixed up with [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/)!), nothing is simpler and more core to the agile values than TBD: Just push to `main` (or `master` if you aren't up with the times) and trust your tooling to handle it.

_Note that even Vincent Driessen, the original conceiver of GitFlow, nowadays actively discourages the use of GitFlow in modern circumstances._

The pros and cons of TBD are of course only fully visible when seen together with other practices and tools you have. There needs to be certain maturity before doing this while remaining safe. This project should represent perfectly valid conditions for which TBD can be used instead of less-agile strategies.

**🎯 Example 2**: To truly support TBD, I've added [husky](https://github.com/typicode/husky) to run pre-commit hooks for various activities, such as testing. This way we get to know even before the code is "sent off" if it's in shape to reach the CI stage.

Read more about Git workflow anti-patterns at [Jason Swett's blog](https://www.codewithjason.com/git-workflow-anti-patterns/).

_Tip: You can usually set up various branching strategies and restrictions in your CI tool, to effectively require TBD._
