# PRzilla MCP connector

Search PRzilla's public CrossFit and HYROX workout and movement library from an AI assistant.

This repository contains registration metadata and setup documentation for the hosted connector. The PRzilla application and catalog implementation are maintained separately.

## Connect

- **Endpoint:** `https://przilla.app/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** OAuth with a PRzilla account
- **Website:** [przilla.app](https://przilla.app)

Add the endpoint as a custom remote MCP server in an assistant that supports Streamable HTTP and OAuth. Sign in to PRzilla, review the catalog permission, and choose **Allow catalog access**. Enable its read-only tools and select PRzilla when asking a catalog question. Existing connections configured without authentication must reconnect. Availability depends on the assistant's plan and administrator settings.

The permission screen shows the account's lookup allowance. Reconnecting or adding another assistant does not reset it. Manage and revoke access at [AI connections](https://przilla.app/connectors). The mobile app's offline catalog is unchanged.

The endpoint accepts JSON-RPC POST requests. Opening it as a web page returns HTTP 405.

## Authentication and health checks

Unauthenticated MCP requests return HTTP 401 with a `WWW-Authenticate` header pointing to [protected-resource metadata](https://przilla.app/.well-known/oauth-protected-resource/mcp). [Authorization-server metadata](https://przilla.app/.well-known/oauth-authorization-server/api/auth) describes the OAuth endpoints. Catalog access requires the `catalog:read` scope; `offline_access` permits token refresh.

Directory health checks must use an authorized OAuth test profile. On Glama, configure **Admin → Test Profile → OAuth 2.0**, authorize a dedicated PRzilla test account, and run **Test Connection**. Registering an OAuth client alone does not authorize it to access the catalog. Keep credentials in the directory's private test profile.

## Available tools

- `search_workouts`: Find public workouts by name, movements, and optional catalog category.
- `get_workout`: Read a named workout's prescription, movements, available scaling guidance, and benchmark targets.
- `search_movements`: Look up movement technique, equipment, easier variations, available demonstration videos, and example workouts.

Input and output schemas are advertised by the server through authenticated `tools/list` requests.

Search results are fuzzy summaries. Fetch the full workout before judging whether it includes a movement or quoting its prescription or time cap. Performance targets are catalog benchmarks, not personal results or official competition rankings. A movement's example-workout links lead to workouts, not movement profile pages.

## Example questions

- What's the Fran workout and what's a good time?
- Find Hero WODs with rope climbs.
- How do I scale Murph for a beginner?
- What's a thruster and what can I do instead?
- Which HYROX formats are in the catalog?

## Scope and privacy

The connector reads public catalog data. It cannot access personal history or custom workouts, save scores, create personalized training plans, or change accounts.

Lookup terms are sent to PRzilla. See the [privacy policy](https://przilla.app/privacy) for data handling and the [terms of use](https://przilla.app/terms). The assistant provider's own privacy policy governs conversations with that assistant.

## Support

Contact [hello@przilla.app](mailto:hello@przilla.app) with the assistant name and error message. Do not include credentials or private training records.

## Registry publication

`server.json` describes the production endpoint for the official MCP Registry. The manual GitHub Actions workflow verifies the publisher release, checks the live authorization challenge and discovery metadata, validates the manifest, and publishes using GitHub OIDC. It requires no stored registry token. This publication check does not establish authenticated tool execution or assistant compatibility.
