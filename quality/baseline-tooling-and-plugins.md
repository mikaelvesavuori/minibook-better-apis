# 🧰 Baseline tooling and plugins

This project uses two very common—but hugely effective—tools, namely [ESLint](https://eslint.org) and [Prettier](https://prettier.io). These two ensure that you have a baseline, pluggable way of ensuring similar standards (and automation of them) across a team. Really, the first thing you want to make sure of is that the code looks and reads the same, regardless of who wrote it. Using these, now you can.

_Don't forget to enable "fix on save"! Also consider the VS Code plugins for_ [_ESLint_](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) _and_ [_Prettier_](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)_._

When it comes to more IDE-centric plugins in the security department, I highly recommend the [Snyk Vulnerability Scanner](https://marketplace.visualstudio.com/items?itemName=snyk-security.snyk-vulnerability-scanner) (the successor to [vulncost](https://github.com/snyk/vulncost)) for [Visual Studio Code](https://code.visualstudio.com). Other nice ones include:

* [Abracadabra, refactor this!](https://marketplace.visualstudio.com/items?itemName=nicoespeon.abracadabra)
* [Checkov](https://marketplace.visualstudio.com/items?itemName=Bridgecrew.checkov)
* [DevSkim](https://marketplace.visualstudio.com/items?itemName=MS-CST-E.vscode-devskim)
* [OpenAPI (Swagger) Editor](https://marketplace.visualstudio.com/items?itemName=42Crunch.vscode-openapi)

**🎯 Example**: You'll see that this project has configurations files such as `.eslintrc` and `.prettierrc` laying around. I'd not write many lines without those tools around.
