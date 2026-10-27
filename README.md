# PRzilla MCP connector

Search PRzilla's public CrossFit and HYROX workout and movement library from an AI assistant.

This repository contains registration metadata and setup documentation for the hosted connector. The PRzilla application and catalog implementation are maintained separately.

## Connect

- **Endpoint:** `https://przilla.app/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** None
- **Website:** [przilla.app](https://przilla.app)

Add the endpoint as a custom remote MCP server in an assistant that supports it. Enable its read-only tools and select PRzilla when asking a catalog question. No PRzilla account or API key is required. Availability depends on the assistant's plan and administrator settings.

The endpoint accepts JSON-RPC POST requests. Opening it as a web page returns HTTP 405.

## Available tools

- `search_workouts`: Find public workouts by name, movements, and optional catalog category.
- `get_workout`: Read a named workout's prescription, movements, available scaling guidance, and benchmark targets.
- `search_movements`: Look up movement technique, equipment, easier variations, available demonstration videos, and example workouts.

Input and output schemas are advertised by the server through `tools/list`.

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

`server.json` describes the production endpoint for the official MCP Registry. The manual GitHub Actions workflow verifies the publisher release, checks the live connector, validates the manifest, and publishes using GitHub OIDC. It requires no stored registry token.
