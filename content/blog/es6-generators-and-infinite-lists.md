+++
date = "2016-03-24T14:10:00+01:00"
draft = false
title = "ES6 generators and infinite lists"

+++

For the past couple days I've been working on a WebSocket server using [express-ws](https://www.npmjs.com/package/express-ws). While developing locally I needed a way to mock [public and private feeds](https://api.test.nordnet.se/projects/api/wiki/Feed_API_documentation) that our [API](https://api.test.nordnet.se/) provides. Since we are already using mock API server while developing locally, having a mock WebSocket server seemed like a good addition.

WebSocket server should allow client application(s) to login and send subscribe/unsubscribe commands. Once client subscribes to e.g. instrument price feed it should be getting periodic updates to emulate changing last price and tick timestamp (and possibly other fields as well). Well, setting `setInterval()` on each subscription is not a problem. Then I needed a good way to send initial message to the client and generate new price value every time `setInterval()` callback executes. First I wanted to return some sort of object representing price feed message and let it have `mutate()` method that would update its values. Seemed wrong way to do it. Another option is to have an initial message and pass it to separate `mutate()` function that should be pure and should return new instance with new values. Better, but still something was itching in the back of my brain. And then I realized that what I was looking for was [generators](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/function*). This is the code I ended up with for now:

```
export function* price(instrument) {
  const message = initialMessage(instrument);
  while (true) {
    yield message;
    message.data = mutate(message.data);
  }
}
```

I should probably refactor it to generate new instance of `message` instead of only replacing `data` property, but that can be done later. Code that uses that generator is quite simple as well:

```
const prices = price(message.args);
client(prices.next().value);
subscriptions[key] = interval(() => client(prices.next().value), 1000);
```

Where `client` is a wrapper around `ws.send()` and `interval` is a wrapper around `setInterval()` that returns a callback for clearing that interval.

So far I'm quite happy with implementation and how generators allowed me to solve periodic price updates problem. Essentially, I've ended up with implementation of an infinite list. I should try using them more in other projects. I guess a good experiment would be replacing promises with generators for async behavior.
