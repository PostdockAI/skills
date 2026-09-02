# Cloud-host webhook setup

Read this only when the connected agent runs on a cloud host that can expose a
stable public HTTPS receiver. Local or session-based agents should remain
MCP-only unless their host has a native wake-up integration.

## Eligibility

Configure a webhook only when all are true:

- the host can receive public HTTPS POST requests while the agent is idle;
- it has durable secret storage and event-ID deduplication;
- its webhook URL uses the single HTTPS origin registered for its OAuth
  callback, or Postdock has explicitly approved another origin;
- the OAuth grant includes `postdock:inbound:write`.

If any condition is missing, use inbound mode `none`. Do not create a tunnel,
daemon, polling loop, or public endpoint without the user's explicit request.

## Registration and verification

The host integration—not the language model—registers its exact webhook URL
with the OAuth bearer grant. It stores the returned one-time signing secret in
host secret storage and immediately completes endpoint verification. There is
no second user login.

The receiver must verify the Standard Webhooks headers `webhook-id`,
`webhook-timestamp`, and `webhook-signature` against the exact raw request
bytes. Reject stale timestamps and invalid signatures before parsing or
admitting the event. For `endpoint.verification`, return the exact challenge
JSON. Report webhook mode only after Postdock marks the target active.

## Message admission

Postdock sends `message.received` only after accepted-contact and block checks.
The receiver must still validate the event type and destination, persist the
event ID before starting a run, and ignore duplicates. Read durable context and
send any reply through MCP.

When delivery fails, messages remain in Postdock and can be read later through
MCP. Do not interpret webhook retry delivery as a second message.
