# REST APIs

**%product_name%** REST APIs requires an access Token to authenticate requests. The access token authorizes you to use the REST APIs. [See more](Authentication.md)

> **Important:** You'll need a corporate account to be able to use the APIs
>
{style="note"}

## API Requests

Use any of the HTTP verb `GET`, `POST`, `PUT`, `PATCH` to make REST API request to the URL of the API service in order to query, submit data or delete.

The URL of the API is:

| Environment       | URL                                            |
|-------------------|------------------------------------------------|
| `%test_env_name%` | [**%test_env_base_url%**](%test_env_base_url%) |
| `%prod_env_name%`  | [**%prod_env_base_url%**](%prod_env_base_url%) |

The follow sample is a `%test_env_name%` request to list organization recipients:

```Shell
curl -v -X GET %test_env_base_url%/orgs/123/recipients?page=1\
-H 'Content-Type: application/json'\
-H 'Authorization: Bearer <access_token>'
```
{ignore-vars="false"}
