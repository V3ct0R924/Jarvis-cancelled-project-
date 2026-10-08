# Jarvis (Cancelled Project)

## Project Status

This project has been discontinued and is no longer maintained. No further features,
bug fixes, or support are planned.

Anyone wishing to continue development may do so under the MIT License.

## Overview

Jarvis is a personal assistant for Windows that accepts voice and text commands. It
integrates large language model (LLM) providers, Model Context Protocol (MCP) tools,
SQLite-backed local memory, and optional features such as speech synthesis and 3D
printer status monitoring.

## `llm_health.py` Module

`llm_health.py` tracks the temporary availability of LLM providers:

- At startup, optionally sends a minimal request to each configured provider eligible
  for routing.
- When a production request fails, marks that provider unavailable for a cooldown
  period to prevent the router from immediately selecting it again.
- Honors the `Retry-After` response header when provided by the upstream service.
- Applies a longer cooldown to structural failures, such as invalid API credentials
  or an unknown model identifier.
- Stores provider health state in process memory; health state is not persisted across
  runs.

The startup health check is controlled by `JARVIS_HEALTH_CHECK_ON_START`. Cooldown
periods are configured through `JARVIS_HEALTH_COOLDOWN_S` and
`JARVIS_HEALTH_LONG_COOLDOWN_S`.

## Configuration

Use `.env.example` as a reference for the supported environment variables. Copy it to
`.env` and replace the sample values with your own configuration and credentials. Do
not commit or publish API keys.
