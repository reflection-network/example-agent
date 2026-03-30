# example-agent

Minimal reference capsule demonstrating agent.nix usage. This is the public demo agent (Ada).

## What it configures

- Agent name: "Ada"
- Simple system prompt
- Imports agent-nix directly (no adapter)
- Produces base agent.nix outputs: devShell, docker image

## Public examples

When referencing this agent in public content (blog, tweets, docs), use "Agent" — not "Ada" or any real capsule name.

## Switching adapters

The README shows how to switch from base agent-nix to an adapter (adapter-claude or adapter-zeroclaw) by changing only the flake input and the `mkAgent` call path.
