# RESTful-HTTP-Queries

Contains .http files used in VS Code and Visual Studio to perform RESTful queries toward SuperOffice Online tenants.

## Prerequisites

In VS Code, install the [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) extension.

Copy the settings.sample.json into your .vscode folder and rename it to settings.json. Afterwards the folder structure is

RESTful-HTTP-Queries/
|-- .vscode
|---- settings.json
|-- src
|---- *.https files

## Environments

If you want to connect towards different environments you can define them in your settings.json. The sample-files comes with 2 environments, sod and online, but you can freely add more environments to fit your needs.

To switch between environments you can use the shortcut `CTRL` + `Alt` + `E`, or F1, and select from the dropdown.

You can also skip configuring for different environments, and just input values in the $shared section. What you define under $shared will be overruled by environment-specific settings.

The properties are as follows:

- `{{env}}` - The environment to query. Either `sod` or `sod2` or `qaonline` or `stage` or `online` or `online3`.
- `{{access_token}}` - The access token to use for authentication.
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

## How a settings.json should look like

```yaml
{
    "rest-client.environmentVariables": {
        "$shared": {
            "access_token": "",
            "ticket": "",
            "api_url": "",
            "env": "",
            "client_id": "",
            "client_secret": "",
            "refresh_token": "",
            "redirect_uri": "https://devnet-tools.superoffice.com/openid/callback",
            "grant_type": "refresh_token"
        },
        "sod": {
            "access_token": "8A:Cust31998.AcidMdWeI..",
            "ticket": "",
            "api_url": "https://sod2.superoffice.com/Cust31998/api",
            "env": "sod",
            "client_id": "a0a89a62a9...",
            "client_secret": "13f63af902...",
            "refresh_token": "IKT4eiAE...",
        },
        "online": {
            "access_token": "",
            "ticket": "",
            "api_url": "",
            "env": "online",
            "client_id": "",
            "client_secret": "",
            "refresh_token": ""
        }
    }
}
```

Its also possible to copy variables from $shared and use them in an environment:
```yaml
{  
    "rest-client.environmentVariables": {  
        "$shared": {  
            "access_token": "8A:Cust12345.ASqHJbq79KxpwtShL/qVGnMg...",  
            "tenant": "Cust12345",  
            "ticket": "7T:MAAxAGYAMQBhADIAYQBhADgANgBlADgAYwB...",  
            "api_url": "https://sod2.superoffice.com/Cust12345/api",  
            "env": "sod2",  
            "client_id": "4fd6sdf376616343b38d141234567890",  
            "client_secret": "1234567890616343b38d112345678904dfgf",  
            "refresh_token": "DFbK232KPTgLAI9TuSbHkbO2EqMPu2cskdjfhskjsdkfjhsdkfjhskdfh2345",  
            "redirect_uri": "https://devnet-tools.superoffice.com/openid/callback",  
            "grant_type": "refresh_token"  
        },  
        "sod": {  
            "access_token": "{{$shared access_token}}",  
            "tenant": "Cust54321",  
            "ticket": "{{$shared ticket}}",  
            "env": "sod2",  
            "api_url": "{{$shared api_url}}",  
            "client_id": "{{$shared client_id}}",  
            "client_secret": "{{$shared client_secret}}",  
            "refresh_token": "{{$shared refresh_token}}"  
        },  
        "online": {  
            "access_token": "",  
            "tenant": "",  
            "ticket": "",  
            "api_url": "",  
            "env": "",  
            "client_id": "",  
            "client_secret": "",  
            "refresh_token": ""  
        }  
    }  
}  
```
