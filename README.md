# RESTful-HTTP-Queries

Contains .http files used in VS Code and Visual Studio to perform RESTful queries toward SuperOffice Online tenants.

## Prerequisites

In VS Code, install the [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) extension.

Copy the settings.sample.json into your .vscode-folder and replace the variables defined:

- `{{env}}` - The environment to query. Either `sod` or `sod2` or `qaonline` or `stage` or `online` or `online3`.
- `{{token}}` - The access token to use for authentication.
- `{{tenant}}` - The online tenant context identifier to query.
- `{{ticket}}` - The ticket credential to use for queries. Must be used with app_secret.
- `{{clientId}}` - client_id for your application.
- `{{clientSecret}}` - client_secret for your application.
- `{{refresh_token}}` - refresh_token for your application.
- `{{redirectUri}}` - redirect_uri defined for your application.
- `{{grantType}}` - set this to 'refresh_token'

## Authorization usage

Query using access token:

```http
GET https://{{env}}.superoffice.com/{{tenant}}/api/v1/Contact/3 HTTP/1.1
Authorization: Bearer {{token}}
Accept: application/json
```

Query using ticket credential with app secret:

```http
GET https://{{env}}.superoffice.com/{{tenant}}/api/v1/Contact/3 HTTP/1.1
Authorization: SOTicket {{ticket}}
SO-AppToken: {{app_secret}}
Accept: application/json
```

## Get Access Token

To aquire a new accessToken you need to have the following properties defined in your settings.json explained above.

- `{{clientId}}` - client_id for your application.
- `{{clientSecret}}` - client_secret for your application.
- `{{redirectUri}}` - redirect_uri defined for your application.
- `{{grantType}}` - set this to 'refresh_token'

Use the [OIDC.http](./src/OIDC.http) file and aquire a new accessToken. The result contains your `{{token}}` and `{{refresh_token}}`, put those into your settings.json
