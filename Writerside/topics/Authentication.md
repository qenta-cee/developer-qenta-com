# Authentication

**%product_name%** REST APIs requires an `access Token` to authenticate requests. The `access token` authorizes you to use the REST APIs.

To call any endpoint you need to include the `access token` in the `Authorization` header with the `Bearer ` prefix.

## Getting an access token

To generate an `access token`, you need to log into the [ProWallet](%prowallet_url%), and follow the following instructions:

> Use your organization credentials to log into ProWallet
>
{style="note"}

<procedure title="Generate access token" collapsible="false">

<step>
Once your logged into the console. Go to the <code>TOKENS</code> option in <code>Manage</code> menu.

<img src="ProWallet_Manage_Token_Contextual.png" alt="Alt Text"/>

</step>

<step>
In the access token management screen click on <code>GENERATE NEW TOKEN</code> button.

<img src="ProWallet_Manage_AccessToken_Screen.png" alt="Alt Text"/>

</step>

<step>
In the form, enter a <strong>token name</strong> and <strong>expiration time</strong>.

<img src="ProWallet_NewAccessToken_Screen.png" alt="Alt Text"/>

<warning>
<p>If the expiration time is not set, then the token will expire in 360 days</p>
</warning>

</step>

<step>
Once the toke is generated, you could copy it and use it for the API calls

<img src="ProWallet_AccessToken_SuccessCreated.png" alt="Alt Text"/>

</step>

</procedure>

## Using the access token

With the `access token` you are ready to make the API call in any programming language. The following examples show you how to use your access token using cURL.

```Shell
curl -v -X POST 'https://api.gcoin.com/orgs/123/recipients'\
 -H 'Content-Type: application/json'\
 -H 'Authorization: Bearer <access_token>'\
 -d '{"field" : "value"}'
```