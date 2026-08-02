---
name: Connect an agent to Superlist over MCP
description: Connect an MCP-compatible agent to the Superlist hosted MCP server using OAuth 2.0, then confirm the session identity.
api: mcp/superlist-mcp.yml
operations: [whoami]
---

# Connect an agent to Superlist over MCP

Superlist exposes a hosted MCP server at `https://app.superlist.com/mcp` (remote, Streamable HTTP).
Access is authenticated with OAuth 2.0 (authorization_code + PKCE S256, dynamic client registration).

## Steps

1. In the MCP client (Claude Settings → Connectors → Add Connector, or the equivalent in ChatGPT/Cursor),
   add a connector pointing at `https://app.superlist.com/mcp`.
2. The client performs OAuth: it may self-register at `https://app.superlist.com/oauth/register`, then opens
   `https://app.superlist.com/oauth/authorize` in the browser. Sign in directly to Superlist — the agent never
   sees your password.
3. After consent, the client exchanges the code at `https://app.superlist.com/oauth/token` for a bearer token
   and stores it. All tool calls send the token in the `Authorization: Bearer` header.
4. Call `whoami` to confirm the authenticated user and that the connection is live.

## Rules
- If a tool call returns `401 unauthorized` ("Missing authorization header"), the token is missing/expired — re-run the OAuth flow.
- Access is scoped to the data the signed-in account already has, and is revocable from the client's connector settings.
