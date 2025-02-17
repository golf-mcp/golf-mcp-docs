# Authed

OAuth for AI agents. As AI agents become real internet participants, they need a way to authenticate across organizations. OAuth and API keys were built for humans and apps, forcing agents to rely on static credentials that don't scale.

Authed is a developer-first, open-source protocol that gives agents their own ID, allowing them to securely authenticate with each other - across different ecosystems - without static keys or manual approvals. Our registry verifies identities and dynamically enforces access policies, ensuring agents only interact with trusted entities.

No static credentials. No human bottlenecks. Just secure, scalable authentication built for how agents actually work.

## Features

- 🤖 **Agent-to-Agent Communication**: Secure, authenticated communication between independent agents using DPoP-based proof of possession
- 🔐 **Built-in Identity Verification**: Each agent gets a unique, verifiable ID through our registry
- 🚦 **Dynamic Policy Engine**: Enforce access policies ensuring agents only interact with trusted entities
- 🔄 **FastAPI Integration**: Seamless integration with FastAPI applications
- 🛠️ **Developer-First**: Simple SDK and CLI tools for easy integration

## Quick Start

### Installation

```bash
pip install agent-auth
```

### Basic Setup

1. Initialize configuration:
```bash
agent-auth init config
```

2. Create your first agent:
```bash
agent-auth agents create --name my-first-agent
```

### FastAPI Integration

```python
from fastapi import FastAPI, Request
from agent_auth_client import verify_fastapi, AgentAuthManager

app = FastAPI()

# Initialize Authed
auth = AgentAuthManager.initialize(
    registry_url="your-registry-url",
    agent_id="your-agent-id",
    agent_secret="your-agent-secret"
)

# Protected endpoint
@app.post("/secure-endpoint")
@verify_fastapi
async def secure_endpoint(request: Request):
    return {"message": "Authenticated!"}

# Making authenticated requests
@app.get("/call-other-agent")
async def call_other_agent():
    async with auth.secure_request("target-agent-id") as session:
        response = await session.post(
            "http://other-agent/secure-endpoint",
            json={"message": "Hello!"}
        )
    return response.json()
```

### Environment Variables

Configure Authed using environment variables:

```bash
export AGENT_AUTH_REGISTRY_URL="your-registry-url"
export AGENT_AUTH_AGENT_ID="your-agent-id"
export AGENT_AUTH_AGENT_SECRET="your-agent-secret"
```

Then initialize:

```python
auth = AgentAuthManager.from_env()
```

## Use Cases

- **Agent-to-Agent Communication**: Enable secure communication between independent AI agents
- **Transaction Systems**: Build secure transaction protocols between agents with built-in identity verification
- **Access Management**: Implement custom access scopes and permission boundaries for your agents

## Documentation

For comprehensive documentation, visit [docs.authed.com](https://docs.authed.com):

- [Core Concepts](https://docs.authed.com/what-is-authed)
- [Use Cases](https://docs.authed.com/capabilities)
- [SDK Guide](https://docs.authed.com/sdk-guide)
- [CLI Reference](https://docs.authed.com/cli-tools)

## Security

⚠️ **Important**: Never commit agent secrets or provider credentials to version control. Use environment variables or secure secret management solutions in production.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.
