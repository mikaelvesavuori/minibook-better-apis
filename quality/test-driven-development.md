---
description: TODO
---

# 🥼 Test-Driven Development

> The red, green, refactor approach helps developers compartmentalize their focus into three phases:
> Red — think about _what_ you want to develop.
> Green — think about _how_ to make your tests pass.
> Refactor — think about _how_ to improve your existing implementation.

— From [Codeacademy](https://www.codecademy.com/article/tdd-red-green-refactor)

{% hint style='info' %}

Read a more [complete introduction to TDD in TypeScript here](https://khalilstemmler.com/articles/test-driven-development/introduction-to-tdd/).

{% endhint %}

[Test-driven development](https://testdriven.io/test-driven-development/) is a practice that a lot of people swear by. I'm a 50/50 person myself—sometimes it makes sense to me, sometimes I just write the tests after I feel I'm out of the weeds when it comes to the first implementation. No need to be a fundamentalist; be a pragmatist! The important thing is that _there are tests_, not so much how and when they came. I'd still note that for this advice, I will assume that you have some kind of rigid standards—it's just too easy to skip the tests!

However, in case you want to be a good-spirited TDD crusader then I've made it easy to do so.

**🎯 Example**: Just run `npm run test:unit:watch` and Jest will watch your tests and source code. You can also modify what Jest "watches" in the interactive dialog.
