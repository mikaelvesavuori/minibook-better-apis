# 🤖 Synthetic testing

Synthetic testing sounds cool and intriguing but is really no more than a directed stream of traffic from a non-human source, such as a computer.

Above we used a smoke test, which is absolutely a synthetic test, but there is another dimension I want to bring up as well: We can use synthetic testing to continuously verify that our solution keeps performing as expected. In that regard, we can think of it as a kind of "offensive testing" mechanism.

Running synthetic traffic is something that can certainly stress the existing instance, but in our case we could also leverage it during the deployment window (when rolling out a canary deployment) to more fully exercise the system, flushing out any issues. This would also give us a higher probability of hitting any issues—which we may not in a, for example, 10-minute window with low organic traffic.

**🎯 Example**: We can use load testing to run various larger-scale synthetic traffic volumes as one-offs to stress the API. Under `tests/load/k6.js` you can see our [k6](https://k6.io) script that we run in CI. The use-case we have here is to ensure that it functions correctly with a range of inputs rather than just a quick poke-and-feel as with the smoke test.

Synthetics can be run with almost anything: Examples include a regular API client like [Insomnia](https://insomnia.rest), a load testing tool like [k6](https://k6.io) or [Artillery](https://www.artillery.io), a SaaS like [Checkly](https://www.checklyhq.com), or a managed tool like [AWS CloudWatch Synthetics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html). Even `curl` works fine! If you want to set something up to continuously run against your endpoint, I recommend using an easy-to-use tool like CloudWatch Canaries or Checkly, directing them to your endpoint.
