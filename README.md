# RESTful-HTTP-Queries

Contains .http files used in VS Code and Visual Studio to perform RESTful queries toward SuperOffice Online tenants.

## Prerequisites

In VS Code, install the [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) extension.

Copy the settings.sample.json into your .vscode-folder and rename it to settings.json

## Environments

If you want to connect towards different environments you can define them in your settings.json. The sample-files comes with 2 environments, sod and online, but you can freely add more environments to fit your needs.

To switch between environments you can use the shortcut `CTRL` + `Alt` + `E`, or F1, and select from the dropdown.

You can also skip configuring for different environments, and just input values in the $shared section. What you define under $shared will be overruled by environment-specific settings.

The properties are as follows:

- `{{env}}` - The environment to query. Either `sod` or `sod2` or `qaonline` or `stage` or `online` or `online3`.
- `{{access_token}}` - The access token to use for authentication.
- `{{tenant}}` - The online tenant context identifier to query.
- `{{ticket}}` - The ticket credential to use for queries. Must be used with app_secret.
- `{{client_id}}` - client_id for your application.
- `{{client_secret}}` - client_secret for your application.
- `{{refresh_token}}` - refresh_token for your application.
- `{{redirect_uri}}` - redirect_uri defined for your application.
- `{{grant_type}}` - set this to 'refresh_token'

## Get Access Token

To acquire a new accessToken you need to have the following properties defined in your settings.json explained above.

- `{{client_id}}` - client_id for your application.
- `{{client_secret}}` - client_secret for your application.
- `{{redirect_uri}}` - redirect_uri defined for your application.
- `{{grant_type}}` - set this to 'refresh_token'

Use the [OIDC.http](./src/OIDC.http) file and run the `Authorization`-method to acquire a new accessToken. The result contains your `{{access_token}}` and `{{refresh_token}}`, put those into your settings.json.

The result also contains the id_token, which you can decode at (<https://jwt.io>) and select the '<http://schemes.superoffice.net/identity/webapi_url>' property from the payload, this should be put into the  `{{api}}`.

An alternative is to run the `Get State`-method in [OIDC.http](./src/OIDC.http), or simply put the url https://{{env}}.superoffice.com/api/state/{{tenant}} in your browser.
This is setting needed because your tenants API URL might dynamically change, and you need to make sure you are connecting towards the correct API URL or else you could get a `421 Misdirected Request`.

## Authorization usage

Query using access token:

```http
GET {{api_url}}/v1/Contact/3 HTTP/1.1
Authorization: Bearer {{access_token}}
Accept: application/json
```

Query using ticket credential with app secret:

```http
GET {{api_url}}/v1/Contact/3 HTTP/1.1
Authorization: SOTicket {{ticket}}
SO-AppToken: {{app_secret}}
Accept: application/json
```
