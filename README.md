# sbx-kit

Docker Sandbox Kit for Goose -- Spin up pre-configured, docker microvm Goose AI agent environments in seconds.

This repository contains **sandbox specifications** for [Goose](https://github.com/aaif-goose/goose), Block's open-source AI coding agent. Each spec defines a complete, ready-to-run environment with Ollama integration, pre-configured extensions, and sensible defaults -- so you can focus on getting work done instead of setting up infrastructure.

## What's Inside

```
sbx-kit/
├── goose/                # Pinned to a specific Goose version
│   └── spec.yaml
└── goose-latest/         # Always pulls the latest Goose release
    └── spec.yaml
```

### goose/ -- Pinned Release

Uses a fixed download URL for **Goose v1.41.0**. The version won't change unless you update the spec manually. Use this for reproducible, stable environments where you want to control when upgrades happen.

### goose-latest/ -- Rolling Release

Uses the GitHub `latest` redirect URL (`/releases/latest/download/...`) so it always fetches the most recent Goose release at build time. Use this when you want the newest features and don't mind the version changing between sandbox launches.

Both specs share the same configuration otherwise:

- **Connects to Ollama** -- pre-configured to use `deepseek-v4-flash:cloud` via a host-local Ollama instance
- **Enables all core extensions** -- developer tools, skills, memory, code execution, todo management, chat recall, subagent summoning, extension management, and top-of-mind context injection
- **Pre-configures permissions** -- allows shell, edit, write, and code execution without prompting, so the agent can work autonomously
- **Installs git + GitHub CLI** -- ready for version control workflows out of the box
- **Runs in Docker** -- uses [Docker Sandbox Templates](https://docs.docker.com/desktop/features/sandbox/) (`docker/sandbox-templates:shell-docker`) for a clean, isolated environment

## Quick Start

### Prerequisites

- [Docker Desktop](https://docs.docker.com/desktop/) with [Sandbox](https://docs.docker.com/desktop/features/sandbox/) support
- [Ollama](https://ollama.ai/) running locally (or accessible via network)
- The `deepseek-v4-flash:cloud` model pulled in Ollama

Those are the defaults however you CAN change your provider and model to whatever you wish, local models will work.

### Usage

1. **Clone this repo:**

   ```bash
   git clone git@github.com:Xarianne/sbx-kit.git
   cd sbx-kit
   ```

2. **Choose your spec:**

   - `goose/spec.yaml` -- pinned version (v1.41.0)
   - `goose-latest/spec.yaml` -- always the latest release

3. **Launch the sandbox** using Docker Desktop's sandbox feature, pointing it at your chosen `spec.yaml`.

   The spec is a [Docker Sandbox specification](https://docs.docker.com/desktop/features/sandbox/) -- it defines the image, environment variables, commands, and entrypoint for a sandboxed environment. Docker Desktop reads this file and provisions the environment accordingly.

4. **Goose starts automatically** -- the sandbox entrypoint runs `goose`, and you're dropped into an interactive session with the agent.

## Configuration Details

| Setting | Value |
|---|---|
| LLM Provider | Ollama (`deepseek-v4-flash:cloud`) |
| Ollama Host | `http://host.docker.internal:11434` |
| Mode | `approve` (asks before destructive actions) |
| Max Turns | 20 |
| Telemetry | Disabled |

### Extensions Enabled

| Extension | Purpose |
|---|---|
| Developer | File editing and shell command execution |
| Skills | Discover and load skill instructions |
| Memory | Learn user preferences over time |
| Code Execution | Run code inline, saving tokens |
| Todo | Track task progress |
| Chat Recall | Search past conversations for context |
| Summon | Load knowledge and delegate to subagents |
| Extension Manager | Enable/disable extensions at runtime |
| Top of Mind (ToM) | Inject custom context into every turn |

## Customization

To adapt this kit for your own use:

- **Change the model** -- edit `providers.ollama.model` in `spec.yaml`
- **Adjust autonomy** -- set `GOOSE_MODE` to `auto` for fully autonomous operation, or `chat` for a conversational style
- **Add extensions** -- the spec uses the latest bundled extensions; add more by including them in the `extensions` section
- **Tweak permissions** -- modify `permission.yaml` to always allow, ask before, or never allow specific tools

## Why sbx-kit?

Goose doesn't ship with an official sandbox template. If you want to run it in an isolated environment, you have to build one yourself -- installing the binary, writing a config file, configuring a provider, setting up permissions, and enabling extensions. That's a lot of boilerplate for what should be a one-liner.

**sbx-kit** packages all of that into a single, portable [Docker Sandbox spec](https://docs.docker.com/desktop/features/sandbox/). It's ideal for:

- Experimenting with Goose in a clean, disposable environment
- Onboarding new team members to AI-assisted development
- Reproducible workflows -- the same environment every time
- Sandboxed execution -- Goose runs in a Docker sandbox, isolated from your host

## Related

- [Goose](https://github.com/aaif-goose/goose) -- The AI coding agent this kit is built for
- [Docker Sandbox](https://docs.docker.com/desktop/features/sandbox/) -- The sandbox technology this spec targets
- [Ollama](https://ollama.ai/) -- Local LLM runner
- [AAIF](https://github.com/aaif-goose) -- Agentic AI Foundation

## License

MIT -- see [LICENSE](LICENSE) for details.
