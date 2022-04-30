---
description: TODO
---

# 📝 Release versioned software and produce release notes

Each release should be uniquely versioned. We should also keep release notes, but it's one of those things that can be easy to miss.

**🎯 Example**: Instead of writing manual release notes, we use [Standard Version](https://github.com/conventional-changelog/standard-version) to output them for us from our commits into the `CHANGELOG.md` file. Practically, this works by running `npm run release`.
