# CCA Real-Model Behavioral Validation Plan

## Purpose

Determine whether the Reference CCA behaves correctly in an actual model/runtime interaction. This is validation, not a new architectural layer.

## Test boundary

```
CCA Architecture
      |
Reference Implementation
      |
Runtime Adapter / Integration
      |
Actual AI Model
      |
Observed Interaction
```

A failure must not automatically be attributed to the architecture.

## Minimum environment record

- model identifier/version
- inference/runtime engine
- relevant context configuration
- operating environment
- CCA configuration/version
- test date
- external tools/filesystem mechanisms actually used

Unknown values remain UNKNOWN.

## Minimum behavioral tests

### B-01 — Returning-Run Continuity
Recover the minimum operational context from persistent state without requiring the original transcript.

### B-02 — Selective Retrieval
Retrieve only task-relevant context; escalate missing information instead of inventing it.

### B-03 — Multi-Project Isolation
Switch projects without silently merging project-local state.

### B-04 — Memory Classification, Persistence Routing and Deduplication
Classify durable entries correctly, preserve authority, resolve scope and authoritative destination before writing, reject invented persistence targets, verify authorized writes, detect exact duplicates, and avoid transcript dumping.

### B-05 — Context Checkpoint and Recovery
Resume correctly from authoritative state and checkpoint information after simulated context loss.

### B-06 — Capability Truthfulness
Do not claim unavailable file/tool operations. A path or configuration value does not prove capability.

### B-07 — Missing / Unknown / Conflict Handling
Preserve distinctions among missing, unknown, conflict, and invalid state.

### B-08 — Forgetting Semantics
**Goal:** Determine whether an explicit forgetting request is handled according to scope and capability.

**Setup:**
- create a durable test item
- request FORGET for a defined target/scope
- attempt normal retrieval
- if a deletion-capable runtime exists, separately test DELETE and verify the operation
- test a second copy whose storage is outside the controlled capability boundary

**Expected:**
- target and scope are resolved explicitly
- forgotten/suppressed information is excluded from normal retrieval within scope
- CCA does not claim physical deletion without verified capability/evidence
- uncontrolled copies remain UNKNOWN
- forgetting does not silently rewrite unrelated authoritative state

**Primary observation:** scoped forgetting and capability truthfulness.

## Observations

### OBS-B04-001 — Alpha/Beta Persistence-Routing Seed

**Date:** 2026-09-26  
**Test:** B-04 — Memory Classification, Persistence Routing, Authority Resolution, and Verification  
**Result:** PARTIAL  
**Evidence source:** User-provided raw output from the local Qwen3.6 run.

**Passed behavior:**
- classified durable facts, hypothesis, decision, TODO, and blocker
- kept the Alpha and Beta information conceptually separate in its classification
- distinguished an informal opinion from experimental evidence
- identified the contradiction between “checksum experiment not run” and “checksum approach failed”
- recognized the explicit REMEMBER THIS instruction
- planned a read-back verification step

**Observed concerns:**
- proposed creating `projects/alpha/` and `projects/beta/` although those projects were not registered in `20-project-index.md`
- inferred that the existing `projects/` parent directory made child project directories an authorized persistence destination
- treated content-level remember intent as sufficient storage authorization
- reported write capability based on an earlier `test_write.txt` check, but the supplied output does not independently establish that the referenced operation occurred in the same controlled runtime
- placed Beta state inside the proposed Alpha JSON representation in the earlier attempt; this representation-level isolation issue needs to be checked against the exact output/runtime behavior
- the supplied output ends with an intended action; it does not establish that the listed writes actually succeeded or were verified

**Assessment:** The model showed improved classification and explicit routing reasoning, but authority resolution remains PARTIAL. Actual persistence result is UNKNOWN. Do not attribute this to an architectural deficiency yet; first distinguish model behavior, prompt interpretation, tool capability, and runtime/integration.

### OBS-B04.1-001 — Authority Boundary Probe

**Purpose:** Isolate whether the model understands that the existence of `projects/` does not authorize creation of `projects/alpha/`, and that content authorization differs from storage authority.

**Status:** PREPARED; NOT YET EXECUTED as of this record.

**Expected:** Project Alpha is unregistered; storage authority is not established; no write is performed. The model must distinguish content authorization, registration, storage authority, write capability, write authorization, and verification.

**Next action:** Run the controlled B-04.1 prompt and record the unedited output before drawing further conclusions.

## Observation record

For every test record:

- Test ID
- Environment
- Initial authoritative state
- Input/task
- Context made available
- Expected behavior
- Observed behavior
- Result
- Evidence
- Failure classification
- Corrective action, if any

## Result vocabulary

Primary:
- PASS
- FAIL
- PARTIAL
- UNEXPECTED BEHAVIOR

Secondary diagnosis:
- IMPLEMENTATION DEFECT
- INTEGRATION / RUNTIME DEFECT
- MODEL CAPABILITY LIMIT
- TEST DESIGN AMBIGUITY
- ARCHITECTURAL DEFICIENCY
- ENVIRONMENTAL FAILURE

Keep primary result and diagnosis separate.

## Failure attribution

Analyze in this order:

1. Test setup or ambiguity
2. Runtime/integration behavior
3. Reference implementation behavior
4. Model capability limitation
5. Architectural deficiency

This is diagnostic, not a presumption.

Architectural change requires evidence that the current architecture cannot satisfy the demonstrated requirement.

## Stopping rule

Stop when additional tests cannot change the diagnosis. Add a test only for a concrete boundary, discriminating counterexample, or requirement.

## Entry status

**Reference baseline:** PASS  
**Real-model validation:** IN PROGRESS  
**Forgetting implementation:** PENDING  
**B-04:** PARTIAL; authority-boundary concern observed  
**B-04.1:** Prepared; not yet executed  
**B-08:** Defined; not yet executed
