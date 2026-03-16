# -kapnet

<pre>
  BIP: TBD
  Layer: Applications
  Title: Kapnet Protocol Family and Sub-BIP Index
  Author: TBD
  Comments-URI: TBD
  Status: Draft
  Type: Informational
  Created: 2026-03-16
  License: BSD-2-Clause
  Requires: 3
</pre>

# Abstract

This document introduces Kapnet as a proposed family of Bitcoin-adjacent protocols
and defines a decomposition of that family into separately reviewable sub-BIPs.
The purpose of this document is not to standardize all Kapnet mechanisms in one
proposal, but to provide a structured index for future documents covering message
formats, relay interfaces, proof formats, execution environments, anchoring
templates, and interoperability profiles.

The document distinguishes between:

1. sub-BIPs that are purely descriptive or informational,
2. sub-BIPs that specify overlay behavior external to Bitcoin consensus,
3. sub-BIPs that describe interfaces to existing Bitcoin policy, relay, mining,
   or wallet behavior, and
4. hypothetical sub-BIPs that would require separate scrutiny if they ever
   proposed changes to Bitcoin consensus or standardness policy.

# Copyright

This BIP is licensed under the BSD 2-clause license.

# Motivation

Bitcoin proposals are easier to review when they are narrowly scoped. Existing BIPs
already demonstrate this discipline. For example, Segregated Witness was not
presented as a single undifferentiated document: witness commitments and consensus
rules were specified separately from peer services and separately again from
getblocktemplate updates. Likewise, proposals that introduce new semantics often
benefit from clearly separated sections for rationale, examples, deployment, and
reference implementation.

Kapnet is too broad to be responsibly presented as one standards-track change. It
includes transport conventions, canonical serialization, cross-message dependency
rules, weakwork and auxiliary proof commitments, local replay semantics, higher-level
execution, package formats, relay economics, archival discovery, Nostr projection,
and Bitcoin anchoring surfaces. These subjects have different trust models,
different review audiences, and different implications for existing Bitcoin systems.

The purpose of this document is therefore to define a reviewable decomposition. It
allows the Bitcoin development community to reject, refine, ignore, or selectively
engage with individual Kapnet components without being asked to accept the entire
system as a package deal.

# Introduction

Kapnet, as used in this document, refers to a proposed overlay protocol family that
treats Bitcoin as a settlement and anchoring substrate while defining higher-layer
message objects, ordering surfaces, replay rules, and optional proof and market
mechanisms outside Bitcoin consensus unless explicitly stated otherwise.

This document does not claim that any sub-BIP listed here is accepted, desirable,
deployable, or appropriate for inclusion in Bitcoin Core. It only provides a formal
index, intended repository structure, and dependency map for discussion and review.

# Design Principles

The following principles guide the sub-BIP split:

- One proposal should define one coherent object family or rule surface.
- Bitcoin-facing commitments should be specified separately from overlay-internal
  message semantics.
- Consensus-adjacent ideas, relay policy ideas, and wallet or peer-service ideas
  must not be conflated.
- Informational and process-like repository documents should remain distinct from
  standards-track encodings or deployment proposals.
- Any proposal that would alter Bitcoin consensus or standardness must stand on its
  own and must not be smuggled in under a larger umbrella document.

# Specification

## Repository Layout

The Kapnet repository, if maintained in BIP style for public review, is proposed to
contain:

- one repository-level index BIP,
- one glossary and notation BIP,
- one family of Kapnet-native overlay specifications,
- one family of Bitcoin-interface specifications,
- one family of interoperability profiles, and
- one conformance and test-vector specification set.

This document is the repository-level index.

## Proposal Classes

### Class A: Informational documents

These describe terminology, unit systems, object registries, or interoperability
profiles without proposing Bitcoin consensus changes.

### Class B: Overlay protocol specifications

These define Kapnet-native byte formats, replay semantics, package formats, and
message classes that can in principle exist without any Bitcoin consensus changes.

### Class C: Bitcoin interface specifications

These define how Kapnet data, proofs, or commitments may be represented using
existing Bitcoin transactions, block templates, coinbase commitments, relay
interfaces, or wallet-facing standards, subject to then-current policy and review.

### Class D: Hypothetical consensus or policy proposals

These are not standardized by this document. If any future Kapnet-related idea
requires a Bitcoin soft fork, a relay policy change, or miner template behavior
change, it must be proposed in a dedicated BIP with independent motivation, review,
and deployment analysis.

## Sub-BIP Index

### KAP-000: Terminology, Notation, and Conformance Language
Type: Informational
Status: Draft
Depends on: none

Defines shared terminology, byte order conventions, hash notation, domain
separators, and RFC-2119 style conformance language for the Kapnet family.

### KAP-001: Network Identity and Transport Registry
Type: Informational or Standards Track (overlay)
Status: Draft
Depends on: KAP-000

Defines network identifiers, transport names, default service classes, and a
registry for overlay transport capabilities.

### KAP-002: Node Handshake and Capability Discovery
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-000, KAP-001

Defines version negotiation, capability advertisement, service flags, keepalive
behavior, and disconnect semantics.

### KAP-003: Canonical Serialization for Kapnet Objects
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-000

Defines canonical binary encodings, integer formats, object tags, extension
sections, and rejection of non-canonical encodings.

### KAP-010: TXXM Envelope Format
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-003

Defines the top-level Transaction CrossMail envelope, including addressing,
payload commitments, signatures, validity horizons, and dependency references.

### KAP-011: TXXM Message Classes and Canonical Payload Families
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-003, KAP-010

Defines standard payload families such as submission, validation, vote, governance
action, market object, relay receipt, and checkpoint attestation.

### KAP-012: Dependency Graph and Replay Semantics
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-010, KAP-011

Defines parent references, causal ordering, conflict sets, orphan handling,
replacement semantics, and deterministic replay requirements.

### KAP-013: Blob and Attachment Commitments
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-003, KAP-010

Defines content-addressed attachments, chunking, merkle commitments, retrieval
references, and size thresholds for inline versus externalized content.

### KAP-020: Weakwork Claim Object and Magic-Byte Commitment Format
Type: Standards Track (overlay or interface)
Status: Draft
Depends on: KAP-003, KAP-010

Defines a serialized weakwork claim object and a magic-byte commitment format for
representing such claims in overlay messages or Bitcoin-facing carrier surfaces.

### KAP-021: Weakwork Validation and Scoring Rules
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-020

Defines admissibility, freshness, nonce-domain separation, anti-reuse rules, score
computation, and tie-breaking for weakwork claims.

### KAP-022: Weakwork Markets and Transfer Receipts
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-020, KAP-021

Defines quote, reservation, assignment, fulfillment, cancellation, and fraud-proof
objects for transferable weakwork markets.

### KAP-030: Heavy Auxiliary Proof-of-Work Commitment Format
Type: Informational or Standards Track (interface)
Status: Draft
Depends on: KAP-003, KAP-020

Defines a proof structure linking Kapnet claims to Bitcoin block headers, coinbase
commitments, or other permitted parent-chain evidence.

### KAP-031: Heavy Auxiliary Proof Contribution Rules
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-021, KAP-030, KAP-050, KAP-051

Defines how heavy auxiliary proofs affect ordering, weighting, reorg handling, and
checkpoint selection inside the overlay.

### KAP-040: AggWit Aggregate Witness Container
Type: Informational or Standards Track (interface)
Status: Draft
Depends on: KAP-003

Defines an aggregate witness data structure, membership proofs, pruning rules, and
canonical encodings.

### KAP-041: AggWit Policy Gradients and Incentive Surface
Type: Informational
Status: Draft
Depends on: KAP-040

Defines optional classification and policy heuristics associated with AggWit-aware
implementations. This document is explicitly non-consensus unless superseded by a
dedicated Bitcoin proposal.

### KAP-050: Braid Structure and Heaviest-Braid Selection
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-011, KAP-012, KAP-021

Defines concurrent ordering, append rules, conflict partitions, braid-tip
selection, and pruning behavior.

### KAP-051: Knot Formation and Checkpointing
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-050, KAP-052

Defines checkpoint intervals, knot proposal and attestation objects, state-root
derivation, challenge windows, and rollback bounds.

### KAP-052: State Commitment Tree
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-003

Defines the persistent state tree, namespace-separated roots, inclusion proofs,
non-inclusion proofs, and deterministic update ordering.

### KAP-060: KScript Virtual Machine Core
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-003, KAP-052

Defines the KScript machine model, stack behavior, arithmetic semantics, halt
conditions, and execution-result encoding.

### KAP-061: KScript Resource Bounds and Determinism
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-060

Defines instruction limits, stack limits, memory ceilings, deterministic failure
semantics, and validator conformance requirements.

### KAP-062: KScript State Access and Cross-Domain Rules
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-052, KAP-060, KAP-061

Defines readable and writable namespaces, capability-gated access, atomicity
boundaries, cross-domain dependencies, and anti-circularity rules.

### KAP-070: Kapplet Package Format
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-003, KAP-060, KAP-061

Defines package manifests, code hashes, dependency declarations, versioning, and
upgrade or deprecation rules for kapplets.

### KAP-071: Kapplet Runtime Interface
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-062, KAP-070

Defines host calls, event subscriptions, message emission interfaces, storage
bindings, and deterministic runtime restrictions.

### KAP-080: Relay Policy, Admission Control, and Postage Classes
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-002, KAP-010, KAP-021

Defines admission rules, postage classes, storage tiers, fetch pricing, rate
limits, and refusal codes.

### KAP-081: Hedlbit Accounting and Reserve-Time Derivation
Type: Informational or Standards Track (overlay)
Status: Draft
Depends on: KAP-080

Defines the Hedlbit unit, byte pricing semantics, storage duration pricing,
sponsorship flows, refunds, and accounting proofs.

### KAP-090: Provider Lookup and Retrieval Proofs
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-013, KAP-080

Defines provider advertisements, namespace coverage claims, retrieval requests,
retention classes, and proofs of delivery or fraud.

### KAP-100: Kapnet-to-Nostr Event Mapping
Type: Informational or Standards Track (interop)
Status: Draft
Depends on: KAP-010, KAP-011

Defines a projection of Kapnet objects into Nostr event kinds, tags, references,
and relay hints.

### KAP-101: Nostr Interoperability Profile
Type: Informational
Status: Draft
Depends on: KAP-100

Defines minimal client interoperability requirements, fallback behavior, and relay
discovery expectations for Kapnet-aware Nostr software.

### KAP-110: Identity, Keys, and Capability Delegation
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-003

Defines key roles, delegated capabilities, revocation, threshold control, and
namespace authority rules.

### KAP-111: Membership Proofs and Reputation Objects
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-110

Defines membership certificates, validation receipts, reputation checkpoints,
challenge objects, and privilege-tier transitions.

### KAP-120: DAO Object Model and Governance Actions
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-010, KAP-052, KAP-110

Defines DAO manifests, treasury roots, capability tables, voting modes, execution
triggers, and governance action payloads.

### KAP-121: Guild, Room, and Submission Namespaces
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-010, KAP-052, KAP-110, KAP-120

Defines guild, room, and submission objects, namespace derivation, inheritance,
moderation surfaces, and fork or precipitation boundaries.

### KAP-130: Market Objects for Hashproductive Capital, Weakwork, and DAO Ops
Type: Standards Track (overlay)
Status: Draft
Depends on: KAP-022, KAP-081, KAP-120

Defines market objects for weakwork quotes, reserved capacity, DAO payroll,
treasury disbursement, and utility-balance refill flows.

### KAP-131: Units, Measures, and Oracle Interfaces
Type: Informational
Status: Draft
Depends on: KAP-000

Defines canonical units such as sats, bytes, adjusted satdepth, Hedlbits, work
units, storage duration units, and optional oracle interfaces.

### KAP-140: Bitcoin Settlement Hooks and Anchoring Templates
Type: Informational or Standards Track (interface)
Status: Draft
Depends on: KAP-020, KAP-030, KAP-052

Defines OP_RETURN commitments, coinbase commitment templates, settlement batch
encodings, and state-root anchoring surfaces.

### KAP-141: Bitcoin Policy Compatibility and Standardness Considerations
Type: Informational
Status: Draft
Depends on: KAP-140

Describes which Kapnet anchoring patterns are merely overlay-valid, which are
standardness-compatible under current assumptions, which are miner-policy-dependent,
and which would require dedicated Bitcoin proposals.

### KAP-150: Conformance Test Vectors and Validation Corpus
Type: Standards Track (supporting)
Status: Draft
Depends on: KAP-003, KAP-010, KAP-021, KAP-030, KAP-040, KAP-050, KAP-051,
KAP-060, KAP-140

Defines shared valid and invalid examples for serialization, TXXM parsing,
weakwork claims, heavy auxiliary proofs, aggregate witness containers, braid
selection, checkpoint formation, KScript execution, and anchoring templates.

## Dependency Summary

The intended dependency flow is:

- terminology before all other documents,
- canonical serialization before any object format,
- TXXM before message families and replay semantics,
- weakwork objects before weakwork scoring and markets,
- braid ordering before knot checkpointing,
- state commitments before execution semantics,
- KScript core before kapplet packaging,
- relay policy before postage economics,
- Bitcoin anchoring templates before any compatibility analysis,
- conformance vectors only after the underlying formats stabilize.

# Examples

## Example 1: Why splitting is preferred

If a future proposal wanted to define a Bitcoin-facing commitment for a Kapnet
state root, that should not also redefine overlay transport, replay semantics,
market logic, and execution semantics in the same document. A reviewer who only
cares about Bitcoin transaction structure should be able to review the anchoring
template in isolation.

## Example 2: Weakwork-only overlay deployment

An implementation might choose to implement KAP-003, KAP-010, KAP-020, KAP-021,
KAP-050, KAP-051, and KAP-052 while ignoring KScript, kapplets, AggWit, and all
Bitcoin anchoring templates. This document permits such selective implementation.

## Example 3: Bitcoin-interface review boundary

A document describing a coinbase commitment layout for Kapnet proofs may be useful
to miners and template authors even if no one adopts the broader overlay. Such a
document can be discussed on its own merits without assuming acceptance of the
larger Kapnet design.

# Rationale

This structure follows precedent already visible in the BIP repository.

When a proposal introduces a new consensus structure and also needs peer relay
changes and mining-template changes, those concerns are often separated into
different BIPs. That reduces review burden and keeps each document legible to the
relevant audience.

The same applies here. Kapnet contains multiple distinct surfaces:

- data model,
- replay and ordering,
- proof formats,
- execution environment,
- package distribution,
- relay economics,
- archival discovery,
- interoperability mapping,
- Bitcoin anchoring.

These should not be bundled into a single standards-track request to the Bitcoin
development community.

# Backward Compatibility

This document is purely organizational and imposes no compatibility requirements on
existing Bitcoin nodes, wallets, miners, relays, or applications.

Any future sub-BIP that claims compatibility with current Bitcoin software must
state that claim explicitly and justify it independently.

# Reference Implementation

There is no reference implementation for this document. Reference implementations,
if any, belong to the individual sub-BIPs.

# Deployment

This document does not require deployment.

If adopted informally as a repository index, it would serve only as a coordination
document for future drafts.

# Security Considerations

This document introduces no direct security changes. However, it is motivated in
part by a security and review concern: broad, multi-layer proposals are harder to
audit and easier to misunderstand. By forcing decomposition, this document is
intended to reduce specification ambiguity and hidden coupling between unrelated
mechanisms.

# References

- BIP 3, for current BIP process requirements and preamble conventions.
- BIP 141, as an example of a narrowly-scoped consensus structure proposal.
- BIP 144, as an example of peer-services changes separated from a consensus BIP.
- BIP 145, as an example of mining-template updates split into a separate document.
- BIP 112, as an example including specification, examples, deployment, and
  reference implementation sections.
- BIP 341 and BIP 129, as examples of modern document presentation style.

# Acknowledgements

TBD
