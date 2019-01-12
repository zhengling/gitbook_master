# Webhooks

Webhooks allow you to build or set up integrations which subscribe to certain events on GitBook.com. When one of those events is triggered, we’ll send a HTTP POST payload to the webhook’s configured URL.

### Example of Integrations

- [Slack Notifications](https://github.com/GitbookIO/services-slack)

{% method %}
## List webhooks {#list}

List webhooks in a book requires admin" permission.

{% sample lang="http" %}
```
GET /book/:author/:book/webhooks/
```
{% endmethod %}

{% method %}
## Get single hook {#get}

{% sample lang="http" %}
```
GET /book/:author/:book/webhooks/:id
```
{% endmethod %}

{% method %}
## Get deliveries for a hook {#list-deliveries}

GitBook stores deliveries for a period of 30days.

{% sample lang="http" %}
```
GET /book/:author/:book/webhooks/:id/deliveries
```
{% endmethod %}

{% method %}
## Get single delivery {#get-delivery}

Use this method to inspect payload and result of a webhook delivery.

{% sample lang="http" %}
```
GET /book/:author/:book/webhooks/:id/deliveries/:delivery
```
{% endmethod %}

## Receiving webhooks {#receiving}

##### Events

When configuring a webhook, you can choose which events you would like to receive payloads for. You can even opt-in to all current and future events. Only subscribing to the specific events you plan on handling is useful for limiting the number of HTTP requests to your server. You can change the list of subscribed events through the UI anytime.

By default, webhooks are only subscribed to all events. The available events are:

| Name | Description |
| ---- | ----------- |
| `all` | Any time any event is triggered (Wildcard Event). |
| `publish` | Content of the book has been updated. |
| `thread` | Any time a Discussion is opened, closed, or reopened. |
| `thread_comment` | Any time a Discussion or Pull Request is commented on. |

##### Delivery headers

HTTP requests made to your webhook’s configured URL endpoint will contain several special headers:

| Header | Description |
| ------ | ----------- |
| `X-GitBook-Event` | Name of the event that triggered this delivery. |
| `X-GitBook-Signature` | HMAC hex digest of the payload, using the hook’s secret as the key (if configured). |
| `X-GitBook-Delivery` | Unique ID for this delivery. |


Also, the `User-Agent` for the requests will have the prefix `GitBook/`.

##### Example delivery

```
POST /payload HTTP/1.1

Host: localhost:4567
X-GitBook-Delivery: 72d3162e81ab4c9367dc0958
X-GitBook-Event: publish
X-GitBook-Signature: sha1=6adfb183a4a2c94a2f92dab5ade762a47889a5a1
User-Agent: GitBook/10.2.0
Content-Type: application/json
Content-Length: 6615

{
  "payload": {
    ...
  }
}
```
