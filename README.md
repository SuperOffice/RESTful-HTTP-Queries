# RESTful-HTTP-Queries

Contains .http files used in VS Code and Visual Studio to perform RESTful queries toward SuperOffice Online tenants.

## Prerequisites

In VS Code, install the [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) extension.

Replace the parameter values at the top of every file contains placeholders for the following values:

- `{{env}}` - The environment to query. Either `sod` or `sod2` or `qastage` or `qastage2` or `online` or `online3`.
- `{{token}}` - The access token to use for authentication.
- `{{tenant}}` - The online tenant context identifier to query.
- `{{ticket}}` - The ticket credential to use for queries. Must be used with app_secret.
- `{{app_secret}}` - The app secret to use for authentication. Must be used with ticket.

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

Use the [OIDC.http](./src/OIDC.http) file and follow the instructions in the file.
