# Sermo JavaScript Bundle

The Sermo JavaScript bundle can be included on a website and can be interfaced with `window.postMessage` (https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage).
It's a minified bundle that handles the lifecycle of the Sermo chat window.

## How to start a sales chat

Send a message through `window.postMessage` with a payload of this format:

```json
{
  "sender": "WEBSITE_BRAND",
  "receiver": "sermo",
  "event": "START_SALES_CHAT",
  "customerId": "12345678",
  "niSalesLeadId": "lead-123",
  "niSessionId": "session-123",
  "maxZIndex": 1501
}
```

Where the "WEBSITE_BRAND" string should be the brand of the website. For example "comviq". It's used by the bundle to determine in which queue to place the customer.

The value for `customerId` should be provided if the customer is logged in on the website.
Providing the `customerId` when starting the chat makes it so that the Bot doesn't have to ask the customer for it.

The value for `niSalesLeadId` should be the ID of the lead that NowInteract provides to the website.

The value for `niSessionId` should be the ID of the session that NowInteract provides to the website.

The value for `maxZIndex` is used so that we can make sure that the CSS `z-index` we open the iFrame with is the highest on the page.

## What the Sermo Bundle will do

The bundle will listen for messages with the receiver "sermo" and open an iFrame for the customer to `https://sermo.comhem.com`.
A key will be saved in the Session Storage by the bundle to keep track of the state of the chat.
Cookies will be saved for the domain `https://sermo.comhem.com` so that Sermo can identify the customer.

There are also certain events that the bundle will emit for that can be listened to if needed.

### The chat is closed by the customer

When the chat is closed the bundle will clean up the iFrame by itself.
A message will also be sent through `window.postMessage` with this format:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_CLOSED"
}
```

### The chat conversation with the customer has started

Chat conversations are seen as started when the customer has sent their first message.
A message will be sent through `window.postMessage` when the Sermo chat session has started:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_CONVERSATION_STARTED"
}
```

### The chat has been minimized by the customer

A message will be sent through `window.postMessage` when the Sermo chat window has been minimized:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_MINIMIZED"
}
```

### The chat has been maximized by the customer

A message will be sent through `window.postMessage` when the Sermo chat window has been maximized:

```json
{
  "sender": "sermo",
  "receiver": "WEBSITE_BRAND",
  "event": "CHAT_MAXIMIZED"
}
```

