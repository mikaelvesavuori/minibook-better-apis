# API documentation

These apply to the full "finished" state.

## Fake user

Get a fake user.

**Required headers**

**`Authorization`**: `erroruser@company.com` | `legacyuser@company.com` |  `standarduser@company.com` | `betauser@company.com` | `qauser@company.com` | `devuser@company.com` |`devnewfeatureuser@company.com`

**`X-Client-Version`**: `1` | `2`

```bash
GET {{API_URL_BASE}}/shared/fakeUser
```

## Feature toggles

Send the user name (email) of a user to get their feature toggles. An unknown (or missing etc.) user will return default toggles.

```json
POST {{API_URL_BASE}}/shared/featureToggles

{
  "userName": "devnewfeatureuser@company.com"
}
```
