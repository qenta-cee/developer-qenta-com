# Webhooks

**%product_name%** supports webhooks, which allow you to receive real-time HTTP POST notifications when certain events occur in **%product_name%**. Webhooks are particularly useful for integrating **%product_name%** with your own systems, or for integrating with third-party services.

## How to use

To use webhooks, you need add a webhook subscription to your **%product_name%** account. You can do this by using [Webhook management](Webhook-management.md) endpoints.

> The webhooks will send the notification to the URL using the `POST` HTTP verb
>
{style="note"}

## Messages

Webhooks send messages to the URL you specify using the `POST` method. The messages are in JSON format and contain information about the event that triggered the webhook.

For example, when an order is paid by the customer, an event that let your system know that the order has been paid.

When your system receive the message, it must verify that the notification message:

- Comes from **%product_name%**
- Was not altered in transit
- Was intended for your organization
- Has a valid format

Then, the endpoint that attends the URL subscribed must respond with a `200 OK` status code to acknowledge the message.

### Format

The message format is as follows:

<include from="Snippets.topic" element-id="order_event_sample"/>

> The payload content may vary depending on the event that triggered the webhook.
> 
{style="tip"}

## Security

For security, message are sent over HTTPS.