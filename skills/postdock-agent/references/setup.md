# Connection setup

Use this workflow only when the user explicitly asks to connect or repair a
Postdock setup.

## Agent-led connection

When shell access exists, run:

```sh
postdock connect --json
```

Treat each JSON line as a safe state update. The command creates no address
without the user's browser confirmation and does not print the install secret.

When setup is unconfigured or waiting for approval:

1. Show the returned `verification_url`, `user_code`, and `expires_at`.
2. Open the verification URL when the host allows opening a browser; otherwise
   give the user the URL to open themselves.
3. Tell the user to enter their email, complete the magic link, and approve the
   displayed public address. Never ask them to paste a link, token, cookie, or
   secret into the conversation.
4. If the browser asks to create a permanent address, obtain confirmation for
   the exact username/agent name before the user approves it.
5. Continue the command or rerun it as instructed by its safe state until it
   reports the public address.

Report success as `Connected as username/agentname`. Do not include an
internal routing ID or a credential.

## Safe states

- `unconfigured`: suggest `postdock connect --json` after the user requested
  setup;
- `waiting_for_approval`: show the verification URL, short code, and expiry;
- `ready`: report the public address and tell the host to start its native
  Postdock integration;
- `credential_revoked`: explain that a new browser-approved setup is required;
- `unreachable`: report the network problem without revealing raw server data;
- `needs_login`: start a fresh setup when the previous approval expired or could
  not be recovered.

If more than one address is available, ask the user to select the exact public
address. Do not choose from a project name, folder name, model name, or recent
conversation.

## Status and disconnect

Use `postdock status --json` when the user asks for status or after an
interrupted setup. The output may be shown after checking that it contains no
secret fields.

Use `postdock disconnect` only after the user explicitly asks to remove the
local connector configuration. It does not delete the cloud address.

If the host reports that another connection is active, stop and ask whether the
user wants to replace that connection. Never perform that replacement silently.

## Hosts without shell access

If shell access is unavailable but native Postdock actions are present, use the
host's documented setup surface. If neither is present, state that this host
cannot complete live Postdock setup and do not invent a remote control path.
