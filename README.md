# GRC-Aware IR Agent

**An autonomous incident response agent that produces GRC-ready outputs from DFIR findings.**

Submitted to the [FIND EVIL! 2026 Hackathon](https://findevil.devpost.com/) by SANS Institute.

## What it does

On top of a DFIR baseline (Protocol SIFT on the SANS SIFT Workstation), this agent produces in seconds:

1. **Dual-taxonomy technique mapping** — observed behaviors tagged with both MITRE ATT&CK (classic TTPs) and MITRE ATLAS (AI-system attacks)
2. **Paired control impact** — NIST CSF 2.0 for traditional controls; ISO/IEC 42001 for AI governance (clause-level references)
3. **Risk scoring** — asset criticality × technique severity → quantified risk
4. **Incident notification draft** — timing guidance and notification targets

The goal: close the gap between "here's what forensics found" and "here's what GRC/compliance/leadership needs to act on."

## Architecture

Custom MCP (Model Context Protocol) server exposing four typed GRC tools to Claude Code. See [`docs/architecture.md`](docs/architecture.md) *(coming Week 1 Day 4)*.

## Project scope

See [`SCOPE.md`](SCOPE.md) — locked on 2026-04-21.

## Try it out

Instructions for judges coming in Week 7. Target: run on a fresh SIFT Workstation in under 15 minutes.

## Repository structure
