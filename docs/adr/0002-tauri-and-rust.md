# 0002. Tauri and Rust, building on Handy's approach

- **Status:** Accepted
- **Date:** 2026-10-10

## Context

Lax needs deep macOS integration (Accessibility API, ScreenCaptureKit, Vision OCR),
low background resource use, and a settings/memory UI. [Handy](https://github.com/cjpais/Handy)
is an MIT-licensed offline speech-to-text app that already solves several adjacent
problems well: local Whisper, audio capture, global shortcuts, inserting text, and a
menu bar app with settings.

## Decision

Lax is a new [Tauri](https://tauri.app) app: a Rust backend for capture, storage and
model calls, and a React/TypeScript frontend for the UI. We follow Handy's architecture
and reuse its code where it fits, keeping Handy's MIT notice on derived files.
Lax is **not** a fork of Handy.

## Consequences

- Small, fast native binary. Rust's memory safety matters for a program that parses
  untrusted content all day.
- Rust has bindings for the macOS frameworks we need; some FFI code will be required.
- A cross-platform path (Windows, Linux) stays open, even though macOS is the first target.
- Contributors need Rust *and* TypeScript.

## Alternatives considered

- **Swift/SwiftUI:** best macOS integration, but macOS-only and can't reuse Handy's code.
- **Fork Handy:** fastest start, but brings in dictation features Lax doesn't need and
  makes the two projects hard to keep apart.
- **Electron:** heavier memory use for an always-on background app.
