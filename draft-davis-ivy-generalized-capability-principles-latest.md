---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "Generalized Capability Principles"
abbrev: "GenCapPrinc"
category: info

docname: draft-davis-nmop-generalized-capability-principles-latest
submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - capability
 - manifest
 - specification
 - representation
 - occurrence
 - component-system
 - pruning&refactoring
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: USER/REPO
  latest: https://example.com/LATEST

author:
  -
    ins: N. R. Davis
    name: Nigel Robert Davis
    org: Ciena
    email: ndavis@ciena.com

  -
    ins: M. Palmero
    name: Marisol Palmero
    org: Individual
    email: marisolpalmero7@gmail.com

contributor:

  -
    ins: N. Davis
    name: Nigel Davis
    org: Ciena
    email: ndavis@ciena.com


normative:

   whatever:
              title: "whatever"
              date: 2022-04-27
              target: https://.pdf

informative:

   ITU-T G.7711:
              title: "Generic…."
              date: 2022-08-31
              target: https://www.itu.int/rec/T-REC-G.7711/recommendation.asp?lang=en&parent=T-REC-G.7711-202202-I)

   ivy:
              title: "ivy"
              date: 2022-08-31
              target: https:// 3.pdf

   plug:
              title: "plug"
              date: 2022-08-31
              target: https:// 3.pdf

   mobo:
              title: "draft-davis-netmod-modelling-boundaries"
              date: 2022-08-31
              target: https:// 3.pdf

   ONF TR-512:
              title:  "TR-512 Core Information Model (CoreModel) v1.5"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip

   ONF TR-512.A.2:
              title:  "TR-512.A.2 Appendix: Model Structure, Patterns and Architecture"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip
 
   ONF TR-512.8:
              title:  "TR-512.8 Control"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip

   ONF TR-512.7:
              title:  "TR-512.7 Specification"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip


   ONF TAPI:
              title:  "ONF Transport API SDK 2.4.0"
              target: https://github.com/OpenNetworkingFoundation/TAPI/releases/tag/v2.4.0


--- abstract

This document introduces a framework for capability modeling based on the specification and refinement principles established in ITU-T G.7711 Annex G (also published as ONF TR‑512.7 see latest release) and the modeling boundaries work documented in `draft-davis-netmod-modelling-boundaries`. The framework defines how component–system capabilities can be explicitly described and refined via a process of pruning, refactoring, and occurrence formation.

These capability definitions can target detailed operational considerations, system interactions, licensing, abstract product declarations, or sales and marketing. The framework supports modular, layered, and fractal declarations of networked behavior, and provides a foundation for a suite of future IETF drafts aligned with ongoing work on photonic plug manifests, entitlement/licensing, IVY equipment modeling, and related domains.

--- middle

#Terminology
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT"
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in the
document are to be interpreted as described in RFC2119}}.

The following terms abbreviations are used in this document:

* capability: Placeholder
* manifest: Placeholder
* specification: Placeholder
* representation: Placeholder
* occurrence: Placeholder
* component-system: Placeholder
* pruning & refactoring: Placeholder

#Introduction

Currently, capabilities are mainly described loosely in human readable text, where that text is often incomplete, ambiguous or inconsistent. While people make these systems work in practice, the looseness result in errors, inefficiencies and limited reuse.
As automation increases, there is a growing need to enable machine reasoning about the capabilities of network systems and components. While Large Language Models (LLMs) can interpret traditional documentation, there remains a strong need for greater formal rigor and structured representation to improve efficiency and precision. When asked, LLMs indicate that a rigorous model is preferable to loose ambiguous text.
Existing IETF models predominantly focus on configuration, operational state, and telemetry. What is missing is a cohesive framework for expressing what a system *can* do, i.e., its capabilities, in a declarative, structured, and reusable form.
This document introduces the principles for a capability modeling framework grounded in the specification concept established in [ITU-T G.7711] ([ONF TR‑512]). It applies these principles through the lens of the **component–system pattern** from [ONF TR-512.A.2], using the concept of **emergence through recursive narrowing and occurrence formation**. These ideas are extended further by the modeling boundary principles described in [mobo].
The result is a standardized and extensible approach for expressing features, operational constraints, internal dependencies, etc. - separately from instance realizations.
This approach supports capability modeling for any aspect of the controlled networking solution, and is designed to enable capability assembly, dynamic composition, licensing control, and integration with other IETF frameworks such as IVY equipment, photonic plug manifests, and entitlement interfaces. It also supports green initiatives where specific detailed capabilities and their power/thermal implications become critical considerations.

#Problem Statement

Network technologies and management-control frameworks increasingly rely on declarative data models to represent both configuration and operational state. However, these models often lack a principled way to describe the *capabilities* of components and systems—what they are able to support or provide, independent of any particular operational instance. This omission makes it difficult to reason about compatibility, constraint satisfaction, composition, or even basic intent feasibility. Clearly, many of these activities take place prior to the installation of the equipment and indeed determine which equipments are to be planned to be installed. In these cases it is not possible to interogate the actual equipment. 
Whilst knowing the YANG model for the equipment is beneficial, it is not sufficient as the YANG model essentially provides a space within which actual state etc. can be expresses, but it supports all possible combinations. The equipment will be very limited in comparison.
Often it is desirable from a systems operation perspective to reduce the available capability through policy or other mechanisms due to the restrictions of a specific role. This becomes challenging if the base capability of a component is unclear and expressed in a chaotic form.
In practice, five distinct concerns are often conflated, and also not fully expressed, within data models:
- The **generic definition** of a model element or concept (e.g., a termination point) - this is expressed in YANG. It is a very broad definition encompassing all possible opportunities and ofthen many illegal state combinations etc.
- The **capability definition** of a system or component, i.e., what it can support or expose (e.g., by a specific type or role of termination point). This is not expressed fully in YANG. There are both challenges with the expression of base capability and expression of the capability of combinations. This is especially sparce in representation
- The users **policy definition** for system operation - the user may eliminate particular capabilities due to complexity, lack of trust, regulation etc. and will not want them offered or may not want them offered under certain circumstances. The equipment will be expected to behave as if it does not have the capabilities as approproiate.
- The **system combination** where an entity type may play several different roles and in each role may have specific distinct intentional limitations/restrictions.
- The **operational instance**—what is configured or active at a given time.
Without a clear structural separation and with the sparseness of information on specific capabilities, it becomes challenging to formally describe feature constraints, support boundaries, or internal limitations. Implementers resort to informal documentation, code comments, yellow stickies, or out-of-band agreements to capture the intent behind model behavior. This reduces interoperability, increases integration effort, and undermines automation as a result of
- **Ambiguity** between what a model element *is* versus what a system *can support*.
- **Redundancy** and inconsistency in the representation of common constraints (e.g., port types, layering, resource limits).
- **Tooling difficulty** when extracting interoperable subsets of large models or generating technology-specific profiles.
- **Incompatibility** between modular subsystems or plug-ins that must declare and verify their supported features.
Furthermore, current models tend to assume a fixed taxonomy of types and features, rather than supporting a process of recursive refinement. This limits their ability to express how complex capabilities *emerge* through constraint, composition, and modular pruning of more general-purpose constructs.
What is needed is a modeling framework that:
- Allows systems and components to be described in terms of their **capability boundaries**, including **capability interactions** separate from operational state,
- Supports **refinement via pruning and refactoring to yield flexible structural transformation** rather than rigid inheritance or classification,
- Enables **recursive occurrence formation**, where each stage of narrowing produces a usable semantic structure,
- Accommodates **multiple valid refinement paths**, supporting different levels of granularity and domain specificity,
- Provides a **coherent trace** from abstract capability declarations down to deployable or licensable configurations.
This draft introduces such a framework by building on the refinement logic of [ITU-T G.7711]  ([ONF TR-512]) in general and especially the **specification pattern** structures of ITU-T G.7711 Annex G (ONF TR‑512.7) which provides a means of expressing bounded capability envelopes through a formal refinement of generic model elements. This also provides grounding in the recursive occurrence model informed by the component–system pattern [ITU-T G.7711]  ([ONF TR-512.A.2] and modeling boundaries approach [mobo]. This document leverages the foundations laid by [ITU-T G.7711]  ([ONF TR-512]).

Also intent everywhere.... TBC

#Generalized Modeling via Component–System–Specification Refinement

This framework moves away from rigid classification schemes and instead adopts a dynamic, refinement-based approach to modeling. Traditional classification attempts to impose fixed categories onto a system, but this often obscures nuance, variation, and the emergence of intermediate structures that carry operational or architectural significance.

We begin instead with the concept of a **universal component**—a general-purpose structure with maximal capability potential. Through the process of **pruning & refactoring** (constraint-driven refinement), this semantic volume is gradually narrowed, yielding intermediate structures with more sharply defined roles and properties. These refined artifacts are not pre-classified entities, but **emergent forms** that arise naturally at specific “sweat spots” in the refinement trajectory—where the remaining capabilities align with a recognizably useful or interoperable function.

Each such emergent form is treated as an **occurrence**. Occurrences appear at every stage of meaningful refinement including at the level of final implementation instances. At all stages of use the application of properties is via the idea of intent where even the tightest constraint of a single value is essentially a statement of intent (as it is impossible to guarantee that a property will be set). This intent consideration will be dealt with further later in this document.

An LTP (Logical Termination Point) in [ITU-T G.7711] ([ONF TR-512]), for example, is not a primitive class but a pattern that arises from pruning and constraining the universal component until only the semantic envelope of an LTP remains.

To support variation, reusability, and convergence across implementations, each component or system is described not by a single fixed class, but by a **specification**: a constrained and possibly pruned refinement of a more generic model element. This allows the model to express bounded capabilities without requiring full instantiation, enabling tools and orchestrators to reason about compatibility, substitution, and support constraints before deployment.
The specification describes the capabilities of an occurrence in terms of occurrences achieved via similar pruning.
A system spec is a pattern assembly of subtly specialized occurrences at a particular level of specialization arranged in a meaningful structure that yields a relevant behaviour.
The specification of an occurrence is itself a system spec.

The combination of the **component–system pattern** with the **specification refinement pattern** enables a modeling architecture where:

- Systems are recursively composed of components,
- Specifications constrain and refine capabilities at each level,
- Occurrences are layered realizations of specs applied to specific contexts or configurations.

This approach supports **gradual realization**, where capability declarations can progressively transition from abstract to concrete, through intermediate spec refinements and pruning. Each layer of model realization adds specificity—structurally (via system composition), behaviorally (via constraints), and operationally (via mapping to configuration/state models).

This recursive refinement:


#Security Considerations



#Security Considerations

TBD

#IANA Considerations

This document has no IANA actions.



--- back

#Appendix A: Interpretive Notes on Refinement and Occurrence

##A.1 No Single Refinement Path

In this modeling approach, there is no single correct way to refine a universal component. The refinement process supports multiple valid paths, each representing a different semantic purpose, level of granularity, or domain context. What emerges depends not on a fixed taxonomy, but on the alignment of constraints, intent, and reuse patterns.

This enables:
- Coexistence of multiple specification layers derived from the same abstract element,
- Domain-specific “semantic phases” that are meaningful within a particular stack (e.g., optical vs packet),
- Purpose-driven modeling: e.g., one path for plug manifests, another for logical topology.

##A.2 Occurrence at Every Layer

Occurrences are not limited to final instances. Each meaningful stage of refinement produces an occurrence—an intent-aligned, constrained projection of the universal component. Even so-called “instances” are not full realizations, but expressed intent within a given operational context.

##A.3 Sweating Out the Shape

Useful structural forms (e.g., an LTP) are not pre-classified primitives. They *emerge* from the pruning process when remaining capabilities reach a “sweat spot” of balance—enough constraints to be meaningful, but not so much as to be frozen. This allows the model to remain adaptive while still supporting mapping, reasoning, and automation.

##A.4 Classification Considered Harmful

Rigid classification schemes tend to obscure natural emergence and lead to artificial separations. This model rejects top-down typing in favor of bottom-up capability surfacing, grounded in refinement logic. Semantic rigor replaces taxonomic rigidity.



#Acknowledgments

This document has been made with consensus and contributions coming from multiple drafts with different visions. We would like to thank all the participants in the IETF meeting discussions.

{:numbered="false"}
