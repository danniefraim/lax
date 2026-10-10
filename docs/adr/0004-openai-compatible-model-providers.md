# 0004. OpenAI-compatible model providers, local by default

- **Status:** Accepted
- **Date:** 2026-10-10

## Context

The best privacy comes from running models locally, but not everyone has a machine
that can. Someone with a weak laptop who cares about privacy should still be able to
use Lax, for example via a provider that processes data in their own country and
doesn't retain it.

The OpenAI chat completions API has become the de facto standard. Local servers
(Ollama, LM Studio, llama.cpp) and many privacy-focused providers (for example STAIK
and Berget AI in Sweden) implement it.

## Decision

- All model calls go through **one internal provider interface** that speaks the
  OpenAI-compatible API (`/v1/chat/completions`, `/v1/embeddings`).
- Chat and embeddings are configured separately.
- The default is local. Remote providers are opt-in, with the consent, per-task routing,
  redaction and outbox-log rules in [GUARDRAILS.md](../GUARDRAILS.md#3-model-providers).
- No provider-specific SDKs. Provider differences are handled with configuration,
  not code paths.

## Consequences

- Any OpenAI-compatible provider works without code changes.
- We can't use provider-specific features that aren't part of the common API.
- Structured output (JSON) support differs between servers. We validate all output
  ourselves and must not depend on any one server's guarantees.

## Alternatives considered

- **Local only:** simplest privacy story, but excludes everyone without a capable machine.
- **Provider-specific SDKs:** access to more features, but more code, more dependencies
  and more ways for data to leak.
