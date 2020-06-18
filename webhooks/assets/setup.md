# Setup & configure webhooks

## Contents

1. [Permission requirements](#permission-requirements)
1. [Adding a webhook to an App using the Cockpit](#adding-a-webhook-to-an-app-using-the-cockpit)
1. [Adding a webhook programatically](#adding-a-webhook-programatically)
   1. [cURL example](#curl-example)
1. [Configuring event scope using URNs](#configuring-event-scope-using-urns)
   1. [Registering a webhook across multiple Apps](#registering-a-webhook-across-multiple-apps)
   1. [Receiving events only on a sub-segment of a resource](#receiving-events-only-on-a-sub-segment-of-a-resource)


## Permission requirements

Users and developers will need the `edit-app` permission in order to manage an App's webhooks.

The `edit-app` permission is granted to users by the `App Admin` role. See our user documentation for more details on how to assign user rights [here](https://support.allthings.me/hc/en-us/articles/360030404511-How-can-you-assign-rights-).

The `edit-app` permission can also be granted to a developer's OAuth Client. To request this permission, please [contact our support](https://support.allthings.me/hc/en-us/requests/new) and we'll get it set up for you.


## Adding a webhook to an App using the Cockpit

App administrators can add and manage webhooks on their Apps from within the Cockpit's [App Settings section](https://cockpit.allthings.me/apps).




## Adding a webhook programatically

Developers can add webhooks programatically using the GraphQL-based [Webhooks API](../apis/webhooks.md).

A new webhook can be created with the `registerWebhook` mutation:

**Request mutation**
```graphql
mutation RegisterNewWebhook($webhook: RegisterWebhookInput!) {
  registerWebhook(webhook: $webhook) {
    code
    message
    success
  }
}
```

**Request variables**
```js
{
  "webhook": {
    "description": "An example webhook",
    "events": ["ticket.created", "ticket.updated"],
    "url": "https://httpbin.org/status/200",
    "urns": ["urn:allthings:app:<put your app ID here>:*"]
  }
}
```

### cURL example

```sh
curl 'https://webhooks.allthings.me/' \
   -H 'Content-Type: application/json' \
   -H 'Accept: application/json' \
   -H 'Authorization: Bearer <put your authorization token here>' \
   --data-binary '{"query":"mutation RegisterNewWebhook($webhook:RegisterWebhookInput!){registerWebhook(webhook:$webhook){code message success}}","variables":{"webhook":{"description":"An example webhook","events":["ticket.created","ticket.updated"],"url":"https://httpbin.org/status/200","urns":["urn:allthings:app:<put your app ID here>:*"]}}}'
```


## Configuring event scope using URNs

Developers can specify the scope of events they're interested in receiving. This can be done specifing a Uniform Resource Name (URN) filter on a webhook. For example, the default behavior is to set the URN filter on a webhook to the whole App, e.g. by specifying `urn:allthings:app:{{appId here}}:*`. Filtering webhooks with URNs can be useful when you're only interested in events on a sub-segment of a resource.

For example, you may only be interested in events on tickets in the "Damage report" category. To achieve this, create a webhook where the `urns` input field has a value like `["urn:allthings:app:<put your app ID here>:ticket:<your ticket category ID>:*]`

Learn more about URNs [here](../urns.md).

### Registering a webhook across multiple Apps

It is possible for developers to subscribe to events across multiple Apps by providing URNs from multiple Apps. This may be useful when your integration is offered to multiple Allthings customers or across multiple Apps.

We don't currently have a UI which exposes this functionality. If you are interested in registering a single webhook for events across more than one App, please register a webhook programatically using the Webhooks API. If you're an App administrator, please feel free to [contact our support team](https://support.allthings.me/hc/en-us/requests/new) and we'll get it set up for you.


### Receiving events only on a sub-segment of a resource

It is possible to receive events for only a sub-segment of some resources. For example, it is possible to configure a webhook to receive only those events occurring on tickets within a selection of ticket categories.

We don't currently have a UI which exposes this functionality. If you are interested in receiving only a sub-segment of events please register a webhook programatically using the Webhooks API. If you're an App administrator, please [contact our support team](https://support.allthings.me/hc/en-us/requests/new) and we'll get it set up for you.




<br /><br /><br />
➡️ **Next Step:** [Build a webhook](./build-webhooks.md)
