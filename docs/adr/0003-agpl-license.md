# 0003. AGPL-3.0 license

- **Status:** Accepted
- **Date:** 2026-10-10

## Context

Lax exists to keep personal data in the user's hands. A permissive license (MIT) would
allow anyone to take Lax, close it, and build a cloud service that collects people's
screen data, without sharing their changes.

## Decision

Lax is licensed under the **GNU Affero General Public License v3.0**. Anyone may use,
study, change and share it. Anyone who distributes a modified version, or runs one as
a network service, must publish their source code under the same license.

Code derived from MIT-licensed projects (such as Handy) keeps its MIT notice. MIT code
can be included in an AGPL project.

## Consequences

- Improvements made by others stay open and can flow back into Lax.
- Some companies avoid AGPL software. That's acceptable for a personal privacy tool.
- Dependencies must be AGPL-compatible. This is checked automatically in CI with `cargo-deny`.
- Changing the license later requires agreement from every contributor.

## Alternatives considered

- **MIT:** maximally permissive, but allows closed forks and closed hosted versions.
- **GPL-3.0:** requires sharing source for distributed apps, but not for network services.
