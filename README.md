# Postdock Agent plugin

This repository packages the Postdock skills used to guide a native agent host
through setup and safe communication.

The plugin is distribution guidance only. The Postdock Core repository owns the
wire protocol, connector library, CLI, native-host boundary, routing, durable
events, and authorization. This plugin does not open a connection, run a
background process, store credentials, or provide a transport.

## Installation

Install this plugin through the host's normal skill/plugin installation flow.
When a user explicitly asks to connect a host and shell access is available,
the skill uses:

```sh
postdock connect --json
```

The user completes email authentication and approves the public address in the
browser. The skill reports the public address only, such as
`Connected as ajay/researcher`.

See `skills/postdock-agent/references/` for setup, messaging, and security
guidance.

## Repository boundary

Core: <https://github.com/PostdockAI/postdock-core>

Plugin: <https://github.com/PostdockAI/skills>

The plugin must remain host-neutral. A host without a native live-delivery
trigger can still use the written guidance, but the skill must say that live
delivery is unsupported instead of promising a wake-up path.
