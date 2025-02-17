# Agent Authentication Documentation

## Table of Contents
- [Installation](#installation)
- [CLI Setup and Usage](#cli-setup-and-usage)
- [SDK Setup and Usage](#sdk-setup-and-usage)
- [Security Best Practices](#security-best-practices)
- [Error Handling](#error-handling)
- [Troubleshooting](#troubleshooting)

## Installation

Install the package using pip:

```bash
pip install agent-auth
```

Required dependencies will be automatically installed:
- click>=8.0.0
- httpx>=0.24.0
- pydantic>=2.0.0
- cryptography>=41.0.0
- pyjwt>=2.8.0
- websockets>=12.0
- uvicorn[standard]>=0.27.0

## CLI Setup and Usage

The CLI tool provides a command-line interface for managing agents and authentication.

### Initial Configuration

Configure the CLI:
```bash
# Interactive configuration
agent-auth init config

# Non-interactive configuration
agent-auth init config --registry-url <URL> --provider-id <ID> --provider-secret <SECRET>

# Clear configuration
agent-auth init clear [--force]
```

The configuration will be saved in `~/.agent-auth/config.json`.

### CLI Commands

#### Managing Agents

Create a new agent:
```bash
agent-auth agents create --provider-id <PROVIDER_ID> [--name <AGENT_NAME>] [--key-file <KEY_FILE>]
```

Get agent information:
```bash
agent-auth agents info <AGENT_ID>
```

#### Managing Permissions

List agent permissions:
```bash
agent-auth permissions list <AGENT_ID> [--output <FILE>]
```

Clear agent permissions:
```bash
agent-auth permissions clear <AGENT_ID> [--force]
```

#### Managing Keys

Generate a new keypair:
```bash
agent-auth keys generate [--output <FILE>]
```

Show public key from a key file:
```bash
agent-auth keys show-public <KEY_FILE>
```

Show private key from a key file:
```bash
agent-auth keys show-private <KEY_FILE> [--force]
```

#### Viewing Logs

Stream logs in real-time:
```bash
agent-auth logs stream [--provider-id <ID>] [--agent-id <ID>] [--level INFO|WARNING|ERROR] [--event-type <TYPE>] [--output <FILE>]
```

Fetch historical logs:
```bash
agent-auth logs fetch [--provider-id <ID>] [--agent-id <ID>] [--level INFO|WARNING|ERROR] [--event-type <TYPE>] [--from-date <ISO_DATE>] [--limit <NUMBER>] [--output <FILE>]
```

### Environment Variables

The CLI supports configuration via environment variables:
- `AGENT_AUTH_REGISTRY_URL`
- `AGENT_AUTH_PROVIDER_ID`
- `AGENT_AUTH_PROVIDER_SECRET`

## SDK Setup and Usage

### Basic Setup

```python
from agent_auth_client import AgentAuthManager

# Initialize the SDK
auth = AgentAuthManager.initialize(
    registry_url="http://your-registry-url",
    agent_id="your-agent-id",
    agent_secret="your-agent-secret"
)

# Or initialize from environment variables
auth = AgentAuthManager.from_env()
```

### FastAPI Integration

```python
from fastapi import FastAPI
from agent_auth_client import verify_fastapi, protect_requests

app = FastAPI()

# Protect your FastAPI endpoints
@app.get("/secure-endpoint")
@verify_fastapi
async def secure_endpoint(request: Request):
    return {"message": "This endpoint is secure"}

# Make authenticated requests to other agents
@app.get("/call-other-agent")
async def call_other_agent():
    auth = AgentAuthManager.get_instance()
    async with auth.secure_request("target-agent-id") as session:
        response = await session.get("http://other-agent/endpoint")
    return response.json()
```

### Environment Variables for SDK

The SDK supports the following environment variables:
- `AGENT_AUTH_REGISTRY_URL` - Required
- `AGENT_AUTH_AGENT_ID` - For outgoing requests
- `AGENT_AUTH_AGENT_SECRET` - For outgoing requests
- `AGENT_AUTH_PRIVATE_KEY` - For outgoing requests
- `AGENT_AUTH_PUBLIC_KEY` - For incoming request verification

## Security Best Practices

1. Always use HTTPS in production environments
2. Store agent credentials securely
3. Rotate agent credentials regularly
4. Monitor authentication events using the logging system
5. Validate all incoming tokens
6. Use specific agent IDs in permissions
7. Don't expose sensitive data in error messages
8. Keep system clocks synchronized for DPoP validation

## Error Handling

The SDK provides several exception classes for handling different error scenarios:

```python
from agent_auth_client import (
    AgentAuthError,
    AuthenticationError,
    ValidationError,
    DPoPError
)

try:
    # Your authentication code
    pass
except AuthenticationError:
    # Handle authentication failures
    pass
except ValidationError:
    # Handle validation errors
    pass
except DPoPError:
    # Handle DPoP-related errors
    pass
except AgentAuthError:
    # Handle other authentication errors
    pass
```

## Troubleshooting

Common issues and solutions:

1. "Client not registered"
   - Verify that the SDK is initialized with valid provider ID
   - Check if the agent registration was successful

2. "Token verification failed"
   - Ensure the target agent has granted necessary permissions
   - Verify that the token hasn't expired
   - Check if the DPoP proof is valid

3. "Invalid DPoP proof"
   - Ensure system clocks are synchronized
   - Verify that the correct key pair is being used
   - Check if the DPoP token hasn't expired

4. "Agent not found"
   - Verify agent IDs from registration
   - Ensure the agent hasn't been deleted
   - Check if the provider ID is correct

For more detailed logs, use the CLI command:
```bash
agent-auth logs fetch --level ERROR
``` 