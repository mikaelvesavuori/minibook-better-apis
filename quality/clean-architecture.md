# Clean architecture

The second thing, and very much cross-functional in regards to quality and stability, is having an understandable, concise and powerful software architecture.

**🎯 Example 1**: You can see a clear taxonomy for how the overall project and the microservices are organized by simply browsing the folder structure and seeing how code is linked together.

One of several tenets of [Robert Martin's "clean architecture" concept](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) is to produce [acyclic code](https://en.wikipedia.org/wiki/Directed_acyclic_graph). You can see that there are no cyclical relations in the Arkit diagrams above. This, among other touches, means that our code is easy to understand, easy to test and debug, and that it is easy to make stable, almost entirely by just logically organizing the code!

Like Martin, I'm also taking cues from [Domain Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design), where we use the ["entity"](https://khalilstemmler.com/articles/typescript-domain-driven-design/entities/) concept to refer to "rich domain models", as opposed to anemic domain models:

> In Martin’s seminal [Patterns of Enterprise Application Architecture] book (2002), a Domain Model is defined as **'an object model of the domain that incorporates both behavior and data'**. This clearly sets it apart from Entity Objects, which are object representations of only the data stored in a database (relational or not), while the behavior is located in separate classes instead. Note that this divide is not really a layering, it’s just procedures working with pure data structures.

— Source: [Codecentric blog](https://blog.codecentric.de/en/2019/10/ddd-vs-anemic-domain-models/)

[Read more about the Anemic Domain Model anti-pattern on Martin Fowler's site](AnemicDomainModel).

**🎯 Example 2**: While the examples in this project may be a bit contrived (bear in mind that they need to balance simplicity with meaningful examples), you can see how the User entity (at `src/FakeUser/entities/User.ts`) has not just data, but also business logic and internal validation on all the different operations it can perform. There is no need to leak such internal detail anywhere else; the only thing we add to that scenario is that we externalize the validation logic so that those functions can be independently tested (for obvious reasons private class methods are not as easily testable).

To keep it short here, I'll just refer to [Robert Martin's original post on clean architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) and [Clean architecture for the rest of us](https://pusher.com/tutorials/clean-architecture-introduction/) for more details. Also, see [Khalil Stemmler's article on how CA and DDD intersect](https://khalilstemmler.com/articles/software-design-architecture/domain-driven-design-vs-clean-architecture/) if that floats your boat.

Clean architecture isn't a revolutionary concept: it's just the best and most logical realization (I feel) so far for questions around code organization that has lingered for decades.
