# example-agent

Ada — a minimal reference capsule for Reflection agents.

## What it does

This repo demonstrates the simplest possible agent capsule: a single `flake.nix` that declares an agent's identity using the agent.nix schema. Ada is a helpful assistant that responds in the user's language.

## The entire capsule

```nix
{
  inputs.agent-nix.url = "github:reflection-network/agent.nix";

  outputs = { self, agent-nix }:
    agent-nix.lib.mkAgent {
      agent = {
        name = "Ada";
        system-prompt = ''
          You are Ada, a helpful assistant.
          You respond in the same language the user writes to you.
        '';
      };
    };
}
```

## Usage

```bash
# Enter dev shell, inspect agent config
nix develop
agent-info

# Build Docker image
nix build .#docker
docker load < result
docker run --rm ada:latest
```

## Using with an adapter

To give Ada a real backend (Claude Code + Telegram), change the import:

```nix
inputs.adapter-claude.url = "github:reflection-network/adapter-claude";

outputs = { self, adapter-claude }:
  adapter-claude.lib.mkAgent {
    agent = { /* same config */ };
  };
```

Then build and run with credentials:

```bash
nix build .#docker
docker load < result
docker run --rm \
  -e TELEGRAM_BOT_TOKEN=<token> \
  -v ~/.claude/.credentials.json:/home/agent/.claude/.credentials.json \
  ada:latest
```

## Documentation

- [Getting started](https://docs.reflection.network/getting-started) — step-by-step capsule creation
- [Adapters](https://docs.reflection.network/adapters) — connecting to Claude Code + Telegram
