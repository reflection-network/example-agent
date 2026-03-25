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

To give Ada a real backend, change the import to an adapter. The agent config stays the same — only the import changes.

### adapter-claude

Claude Code CLI backend with Telegram long-poll transport:

```nix
inputs.adapter-claude.url = "github:reflection-network/adapter-claude";

outputs = { self, adapter-claude }:
  adapter-claude.lib.mkAgent {
    agent = {
      name = "Ada";
      system-prompt = ''
        You are Ada, a helpful assistant.
        You respond in the same language the user writes to you.
      '';
    };
  };
```

```bash
nix build .#docker
docker load < result
docker run --rm \
  -e TELEGRAM_BOT_TOKEN=<token> \
  -v ~/.claude/.credentials.json:/home/agent/.claude/.credentials.json \
  ada:latest
```

### adapter-zeroclaw

ZeroClaw runtime (Rust binary) with native Telegram channel, 60+ built-in tools, SQLite memory:

```nix
inputs.adapter-zeroclaw.url = "github:reflection-network/adapter-zeroclaw";

outputs = { self, adapter-zeroclaw }:
  adapter-zeroclaw.lib.mkAgent {
    agent = {
      name = "Ada";
      system-prompt = ''
        You are Ada, a helpful assistant.
        You respond in the same language the user writes to you.
      '';
      provider = "claude-code";
      model = "claude-sonnet-4-5-20250929";
      transports.telegram.enable = true;
    };
  };
```

```bash
nix build .#docker
docker load < result
docker run -d --memory 4g \
  -e TELEGRAM_BOT_TOKEN=<token> \
  -v ~/.claude/.credentials.json:/home/agent/.claude/.credentials.json \
  ada:latest
```

See [Adapters](https://docs.reflection.network/adapters) for full details on both adapters, including provider options and schema support.

## Documentation

- [Getting started](https://docs.reflection.network/getting-started) — step-by-step capsule creation
- [Adapters](https://docs.reflection.network/adapters) — available adapters and the adapter pattern
