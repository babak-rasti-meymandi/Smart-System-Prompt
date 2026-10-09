# CCA Project

## Project Identity

**Project Name:** CCA  
**Project Type:** Reference Architecture / Local AI Context and Continuity Architecture  
**Status:** ACTIVE

## Purpose

CCA (Cognitive Continuity Agent) is an architecture for organizing persistent context, project state, user continuity, runtime information, and controlled context retrieval for AI systems.

CCA is an architecture, not a model.

It is designed to remain independent of:

- AI model vendor
- model family
- model size
- hardware
- operating system
- inference runtime
- programming language
- specific implementation technology

## Scope

The project covers the definition, construction, validation, and refinement of the CCA Reference Architecture, including core principles, runtime/capability representation, project registration and state, active context, user continuity, retrieval, memory handling, project switching, checkpointing, controlled execution, and real-model behavioral validation.

## Current Phase

**Reference Architecture Validation**

The architecture baseline, file contracts, Reference Implementation boundary, and seven core implementation responsibilities are defined. The deterministic controlled baseline has passed. Real-model behavioral validation is in progress.

## Current State

Documentation now defines explicit forgetting semantics and persistence routing. The B-04 Alpha/Beta seed test returned PARTIAL: classification and conflict handling were useful, but the model inferred that an existing parent directory permitted creation of unregistered project destinations. The supplied output does not prove that any writes succeeded or were verified.

The next test is B-04.1, a minimal authority-boundary probe. No architectural deficiency has been established; diagnosis remains evidence-driven.

## Architectural Constraint

The Reference CCA remains model-, hardware-, operating-system-, runtime-, and implementation-independent. Runtime-specific capabilities must be established by actual evidence and must not be inferred from paths or configuration text.

Compact and specialized CCA variants remain deferred until the Reference Architecture has been sufficiently validated.
