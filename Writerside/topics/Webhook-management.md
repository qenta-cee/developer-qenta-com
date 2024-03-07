# Webhook Management

**%product_name%** backend uses **webhooks** for event notifications. Webhooks are HTTP callbacks that receive notification messages for events.<br/>
Once you configure a webhook endpoint listener for your system, you can register a webhook subscription, which subscribes the URL listener.

> The webhooks will send the notification to the URL using the `POST` HTTP verb
>
{style="note"}

<api-doc openapi-path="../api/massive_transactions_api-docs.yaml" tag="WebHookSubscription"></api-doc>