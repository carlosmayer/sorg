---
title: Webhooks, Operability, and the Future of Streaming APIs
published_at: 2017-06-06T03:00:11Z
location: San Francisco
hook: TODO
---

The term "webhook" was coined back in 2007 by Jeff Lindsay
as a "hook" (or callback) for the web; meant to be a
general purpose system to allow Internet systems to be
composed in the same spirit as the Unix pipe. By speaking
HTTP and being symmetrical to common HTTP APIs, they were
an elegant answer to a problem without many options at the
time -- WebSockets wouldn't be standardized until 2011 and
would only see practical use much later, and more
contemporary streaming options were still a only speck on
the horizon.

At Stripe, webhooks may be one of the best known features
of our API. They can used to configure multiple receivers
that receive customized sets of events, and they also work
for connected accounts, allowing platforms build on Stripe
to tie into the activity of their users. They're useful and
are a feature that's not going anywhere, but are also far
from perfect, especially from an operational perspective.
In the spirit of avoiding [accidental
evangelism](/accidental-evangelist), lets briefly discuss
whether they're a good pattern for new API providers to
emulate.

## User ergonomics (#user)

While think the stronger case against webhooks can be made
from the perspective of an operator, it's worth noting
first of all that the user and developer experience around
webhooks isn't without its own problems. None of them are
insurmountable, but alternatives might get more things
right more easily.

### Endpoint management (#endpoints)

Getting an HTTP endpoint provisioned to receive a webhook
isn't technically difficult, but it can be occasionally
frustrating.

The classic example is the large enterprise where getting a
new endpoint exposed to the outside world might be a
considerable project involving negiotiations with
infrastructure and security teams. In the worst cases,
webhooks might be wholly incompatible with an
organization's security model where user data is
uncompromisingly kept within a secured perimiter at all
times.

**TODO:** Cartoon of perimiter security.

Development is another difficult case. There's no perfectly
fluid way of getting an endpoint from a locally running
environment exposed for a webhook provider to access.
Workarounds like Ngrok are the best option, but still add a
step and complication that wouldn't be necessary otherwise.

### Uncertain security (#security)

Because webhook endpoints are publicly accessible HTTP
APIs, it's up to providers to build in a security scheme
to ensure that an attacker can't issue malicious requests
containing forged payloads. This is possible with a variety
of techniques:

1. ***Webhook signing:*** Sign webhook payloads and send the
   signature via HTTP header so that users can verify it.
2. ***HTTP authentication:*** Force users to provide HTTP
   basic auth credentials when they configure endpoints to
   receive webhooks.
3. ***API retrieval:*** Provide only an event identifier in
   webhook payload and force recipients to make a
   synchronous API request to get the full message.

Although security is possible, the fundamental problem with
webhooks is that it's difficult as a provide to _ensure_
that your users are following best practices (not the case
for synchronous APIs where you can mandate certain keys and
practices). Of the three options above, only the third
guarantees strong security; even if you provide signatures
you can't know for sure that your users are verifying them,
and if forced to provide HTTP basic auth credentials, many
users will select weak ones.

### Testing UI (#testing)

### Order (#order)

## Operator downsides (#operator)

### Retries (#retries)

### Misbehavior is on the provider (#misbehavior)

### Chattiness and communication efficiency (#chattiness)

## A strength: load balancing (#balancing)

## What's next (#whats-next)

### GraphQL subscriptions (#graphql)

### GRPC streaming RPC (#grpc)

## The future (#future)