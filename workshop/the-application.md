# The application

The demo application provides an API that returns a "fake user":

- The first version (`current`) of the API returns a hardcoded name.
- The second version (`beta`) includes a name and some other fields retrieved from an external API, plus a profile image (of a cat) from another external API.
- Finally, a new feature is developed on top of the second version, using a third external API. This feature is hidden under a feature toggle named `enableNewUserApi`.

More flows and details can be seen in the diagrams below.

The main software components are:

- **Microservice**: The `FakeUserBasic` service, if you want to do the workshop part and have some boilerplate code ready
- **Microservice**: The `FakeUser` service
- **Microservice**: The `FeatureToggles` service

Based on the user toggles, the service calls out to the following external APIs:

- **catApi** @ [https://thatcopy.pw/catapi/rest/](https://thatcopy.pw/catapi/rest/)
- **JSONPlaceholder** @ [https://jsonplaceholder.typicode.com/users](https://jsonplaceholder.typicode.com/users)
- **RandomUser** @ [https://randomuser.me/api/](https://randomuser.me/api/)

The feature toggles are fetched, as has been stated previously, from Mockachino, where you will have to create an endpoint with the toggles payload.
