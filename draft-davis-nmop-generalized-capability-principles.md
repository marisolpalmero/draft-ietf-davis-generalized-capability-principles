---
title: "Generalized Capability Principles"
abbrev: "GenCapPrinc"
docname: draft-davis-nmop-generalized-capability-principles-01
category: info
stand_alone: true

submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
wg: "NMOP"
kw:
 - capability
 - manifest
 - specification
 - representation
 - occurrence
 - component-system
 - pruning&refactoring
venue:
  group: "Network Management Operations"
  type: ""
  mail: "nmop@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/nmop/"
  github: "marisolpalmero/draft-ietf-davis-generalized-capability-principles"
  latest: "https://github.com/marisolpalmero/draft-ietf-davis-generalized-capability-principles/blob/main/draft-davis-nmop-generalized-capability-principles-latest.md"

author:

 -
    ins: N. R. Davis
    name: Nigel Robert Davis
    org: Ciena
    email: ndavis@ciena.com

 -
    ins: C. Cardona
    name: Camilo Cardona
    organization: NTT
    email: "camilo@gin.ntt.net"

 -
    ins: D. Lopez
    name: Diego Lopez
    organization: Telefonica
    email: "diego.r.lopez@telefonica.com"

 -
    ins: M. Palmero
    name: Marisol Palmero
    org: Independent
    email: marisol.ietf@gmail.com


contributor:

  -
    ins: N. Davis
    name: Nigel Davis
    org: Ciena
    email: ndavis@ciena.com


normative:

  RFC2119:
  RFC8174:


informative:

   BaseInventory: I-D.ietf-ivy-network-inventory-yang

   ITU-T_G.7711:
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

   ONF_TR-512:
              title:  "TR-512 Core Information Model (CoreModel) v1.5"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip

   ONF_TR-512.A.2:
              title:  "TR-512.A.2 Appendix: Model Structure, Patterns and Architecture"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip

   ONF_TR-512.8:
              title:  "TR-512.8 Control"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip

   ONF_TR-512.7:
              title:  "TR-512.7 Specification"
              target: https://opennetworking.org/wp-content/uploads/2021/11/TR-512_v1.5_OnfCoreIm-info.zip


   LF_TAPI:
              title:  "Transport API"
              target: https://github.com/Open-Network-Models-and-Interfaces-ONMI/TAPI-Home


--- abstract

This document introduces a framework for capability modeling based on the specification and refinement principles established in ITU-T G.7711 Annex G (also previously published as ONF TR‑512.7. See latest G.7711 release) and the modeling boundaries work documented in `draft-davis-netmod-modelling-boundaries`. The framework defines how component–system capabilities can be explicitly described and refined via a process of pruning, refactoring, and occurrence formation.

This draft version additionally incorporates a sketch of an occurrence-semantics kernel. This kernel is presented here as a continuation of the refinement and occurrence-formation principles already introduced in the previous version, not as a retrospective reinterpretation of them.

These capability definitions can target detailed operational considerations, system interactions, licensing, abstract product declarations, or sales and marketing. The framework supports modular, layered, and fractal declarations of networked behavior, and provides a foundation for a suite of future IETF drafts aligned with ongoing work on photonic plug manifests, entitlement/licensing, IVY equipment modeling, energy/thermal considerations and related domains.

--- middle

#Terminology
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT"
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in the
document are to be interpreted as described in RFC2119}}.

The following terms abbreviations are used in this document:

* capability: What can be achieved by an individual item both alone and in assembly (using the component-system pattern)
* needs: Related to capability, this is what the item, either alone or in assembly, needs to achieve its capabilities
* manifest: A list of essential contents
* specification: A detailed definition of capabilities/needs in terms of opportunities/constraints including the arrangement of essential parts and their interconnectivity in assembly
* representation: An expression of structure and properties from a perspective
* component: A thing defined by a boundary where the internal structure within that boundary is not directly visible but is apparently visible through the behaviour exposed at that boundary
* port: A place on the boundary of a component where interaction with that component is possible. Any occurrence may expose ports, where a port is a place on the boundary of an occurrence where interaction with that occurrence is possible.
* component-system: A pattern that expresses each item as a component where components can be assembled into systems and where a system can be represented as a component where that assembly may be of real things or may be abstractions of the effect of real things.
* occurrence: A thing, the specification of which is a purposeful refinement partially constraining the definition of a broader thing, where a thing is a component, a specification, rules, mappings etc.
* pruning: A process of narrowing of definition by reduction of capabilities
* refactoring: A process of rearranging, splitting and combining representation whilst maintaining semantic validity
* pruning & refactoring: The process that supports intentional progression of refinement from one level of structure of occurrences (e.g., system of components) to the next more specific level of structure of occurrences

The following additional terms are used in this version:
* point: Emergent when two compatible ports join. A point is not itself a separate semantic node or vertex, it is simply the recognition of two ports joined.
* hyperedge fabric: The interconnected collection of occurrence-hyperedges, viewed structurally with occurrence semantics suppressed, e.g., the structural arrangement of components in a system ignoring their function.
* system (structural reading): The same hyperedge fabric as above, viewed with the semantics carried by its occurrence-hyperedges exposed. This aligns with, and does not replace, the existing component-system definition above.
* opportunity region: The set, region or family of opportunities permitted by an occurrence definition, written [[A]] for occurrence A.
* void: A semantic state denoting neutral absence of a surviving contribution for a property, relationship or rule occurrence at a given stage; void merge X = X.
* never: A semantic state denoting that the accumulated hard semantic definition is unsatisfiable; absorbing under merge until explicitly reset or pruned to void.
* merge: Conjunctive combination of two occurrences' opportunity regions: A merge B denotes intersection([[A]], [[B]]).
* refines: A refines B iff [[A]] is a subset of [[B]].
* complete: A boundary-scoped operator that changes omission semantics: inherited properties not stated locally at a complete boundary are actively pruned to void. This removes the need to enumerate all possible semantics to be eliminated. It does not prevent semantics from being added back in later.
* where rule: A hard, conjunctive cross-occurrence constraint; removes opportunities that do not satisfy it and can force a region to never.
* prefer rule: A ranking over opportunities that remain legal after all hard contributions have been applied; never makes an otherwise legal opportunity illegal.
* provenance: The recorded source and derivation path of a contribution, rule or pruning decision, preserved through merge without altering the algebraic result.
* qualified identity: A stable namespace plus local name used to determine when two contributions resolve to the same property, relationship or rule for the purposes of automatic combination.


#Introduction

Currently, capabilities are mainly described loosely in human readable text, where that text is often incomplete, ambiguous or inconsistent. While people make these systems work in practice, the looseness result in errors, inefficiencies and limited reuse.
As automation increases, there is a growing need to enable machine reasoning about the capabilities of network systems and components. While Large Language Models (LLMs) can interpret traditional documentation, there remains a strong need for greater formal rigor and structured representation to improve efficiency and precision. When asked, LLMs indicate that a rigorous model is preferable to loose ambiguous text.
Existing IETF models predominantly focus on configuration, operational state, and telemetry. What is missing is a cohesive framework for expressing what a system *can* do, i.e., its capabilities, in a declarative, structured, and reusable form.
This document introduces the principles for a capability modeling framework grounded in the specification concept established in [ITU-T_G.7711] ([ONF_TR‑512]). It applies these principles through the lens of the **component–system pattern** from [ONF_TR-512.A.2], using the concept of **emergence through recursive narrowing, refactoring and occurrence formation**. These ideas are extended further by the modeling boundary principles described in [mobo]. They are extended further still by an explicit occurrence-semantics kernel sketch, which when complete will supply a formal algebra for combining, ranking and detecting contradiction among capability contributions, and a structural model in which occurrences relate through port-joining (emergent points) rather than informal assembly alone.
The result is a standardized and extensible approach for expressing features, operational constraints, internal dependencies, etc. - separately from instance realizations.
This approach supports capability modeling for any aspect of the controlled networking solution, and is designed to enable capability assembly, dynamic composition, licensing control, and integration with other IETF frameworks such as IVY equipment, photonic plug manifests, and entitlement interfaces. It also supports initiatives focussing on energy/thermal considerations where specific detailed capabilities and their power/thermal implications become critical considerations.

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

A sixth concern compounds the first five even where they are addressed structurally: even a well-separated capability definition is of limited use to automation without an explicit, composable algebra for combining contributions from multiple sources, ranking competing opportunities, and detecting contradiction. In the absence of such an algebra, tools fall back to ad hoc, source-order-dependent combination — effectively re-introducing the chaos this document otherwise removes, one layer up.
Furthermore, current models tend to assume a fixed taxonomy of types and features, rather than supporting a process of recursive refinement. This limits their ability to express how complex capabilities *emerge* through constraint, composition, and modular pruning of more general-purpose constructs.
What is needed is a modeling framework that:
- Allows systems and components to be described in terms of their **capability boundaries**, including **capability interactions** separate from operational state,
- Supports **refinement via pruning and refactoring to yield flexible structural transformation** rather than rigid inheritance or classification,
- Enables **recursive occurrence formation**, where each level of pruning and refactoring produces a usable semantic structure,
- Accommodates **multiple valid refinement paths**, supporting different levels of granularity and domain specificity,
- Provides a **coherent trace** from abstract capability declarations down to deployable or licensable configurations.

- Provides a sketch of an explicit, composable algebra for combining, ranking and detecting contradiction among capability contributions drawn from multiple sources, so that the trace above remains sound under merge rather than only under a single author's intent.
This draft introduces such a framework by building on the refinement logic of [ITU-T_G.7711]  ([ONF_TR-512]) in general and especially the **specification pattern** structures of ITU-T G.7711 Annex G (ONF TR‑512.7) which provides a means of expressing bounded capability envelopes through a formal refinement of generic model elements. This also provides grounding in the recursive occurrence model informed by the component–system pattern [ITU-T_G.7711]  ([ONF_TR-512.A.2] and modeling boundaries approach [mobo]. This document leverages the foundations laid by [ITU-T_G.7711]  ([ONF_TR-512]).

The same expression challenges appear in statements of intent. The process of formulating intent through negotiation and resultant gradual refinement has a similar feel to the degrees of pruning and refactoring of the specification.

#Specification in terms of the Model
The specification of capability should be presented in terms of the terminology of the problem space and hence in terms of the appropriate model. The challenge is determining which model is the "appropriate" model.

An area of the problem space can be described in different ways depending upon what the intention of the model is. There are many ways of representing a semantic space/

Prior to embarking on evaluation of specification of capability, it is important to consider the specific model and how it is structured.

- Focus: Semantic area covered at centre and periphery
- Specialization: Specific detailed focus on an area with rich structure, e.g., PCE, problem analysis, etc.
- Granularity: the “size” of the semantic units (including the depth of recursion of fractal representations)
- Phase: The positioning of the semantic boundaries
- Richness: The detailed coverage within a semantic unit
- Fidelity: Precision v approximation
- Abstraction: Closeness to actual detail
- Maturity: Lifecycle development stage. How stable the model is likely to be. This is primarily about semantics, but also covers syntax.
- Omission: Gaps and missing parts

- Role: Whether a given value in the model is desired, specified, committed, claimed, observed or realised. Exactness of a value is orthogonal to this dimension: a singleton value states no role by itself, and the appropriate model for a given purpose must make clear which role is being characterised.

#Generalized Modeling via Component–System–Specification Refinement
This framework moves away from rigid classification schemes and instead adopts a dynamic, refinement-based approach to modeling. Traditional classification attempts to impose fixed categories onto a system, but this often obscures nuance, variation, and the emergence of intermediate structures that carry operational or architectural significance.

We begin instead with the concept of a **universal component**—a general-purpose structure with maximal capability potential. Through the process of **pruning & refactoring** (constraint-driven refinement), this semantic volume is gradually refined, yielding intermediate structures with more sharply defined roles and properties. These refined artifacts are not pre-classified entities, but **emergent forms** that arise naturally at specific “sweat spots” in the refinement trajectory, where the remaining capabilities align with a recognizably useful or interoperable function.

Each such emergent form is treated as an **occurrence**. Occurrences appear at every stage of meaningful refinement including at the level of final implementation instances. At all stages of use the application of properties is via the idea of intent where even the tightest constraint of a single value is essentially a statement of intent (as it is impossible to guarantee that a property will be set). This intent consideration will be dealt with further later in this document.

An LTP (Logical Termination Point) in [ITU-T_G.7711] ([ONF_TR-512]), for example, is not a primitive class but a pattern that arises from pruning and constraining the universal component until only the semantic envelope of an LTP remains. A TerminationPoint from RFC8345

To support variation, reusability, and convergence across implementations, each component or system is described not by a single fixed class, but by a **specification**: a constrained and possibly pruned refinement of a more general and broader model element. This allows the model to express bounded capabilities without requiring full instantiation, enabling tools and orchestrators to reason about compatibility, substitution, and support constraints before deployment.
The specification describes the capabilities of an occurrence in terms of occurrences achieved via similar pruning.
A system spec is a pattern assembly of subtly specialized occurrences at a particular level of specialization arranged in a meaningful structure that yields a relevant behaviour.
The specification of an occurrence is itself a system spec.

The combination of the **component–system pattern** with the **specification refinement pattern** enables a modeling architecture where:

- Systems are recursively composed of components,
- Specifications constrain and refine capabilities at each level,
- Occurrences are layered realizations of specs applied to specific contexts or configurations.

This approach supports **gradual realization**, where capability declarations can progressively transition from abstract to concrete, through intermediate spec refinements and pruning. Each layer of model realization adds specificity—structurally (via system composition), behaviorally (via constraints), and operationally (via mapping to configuration/state models).

A specification may provide explicit definifinition of a property as discussed above but it may also refer to one or more other specification(s). For example a specification may include a set of properties specified elsewhere. It may also define a property that is an enumeration of literals or identifies where those literal values or identify values are actually references to other specifications that provide deeper detail.

In an ideal environment, there is an ecosystem of specificactions each providing interrelated detail to fully define the semantics. The ecosystem would include specifications from standards bodies providing the definition of a network protocol that can be interpreted by an AI component such that the abstracted effect on the solution can be fully understood and simulated/emulated. Any detected conditions would be understood in terms of the protocol and hence the implications of the condition detected in terms of the carried signal can be fully understood.

In this ideal environment, the specification would fully capture all non-failure case behaviours of a component (and potentially some common failure cases) and the component would be designed internally to "guarantee" these behaviours (it would be engineered with appropriate control structures that would bound its behaviour). These specifications, although abstractions would often be highly complex (consider the specification of a CPU for example), but would be less overwhelming in detail and stated in terms of intentional behaviour as opposed to behaviour of the parts. The specification is a statement of the effects of the assembly of detailed parts (see definition of component).

The specification of capability provides a stabilising layer reducing the reasoning required to build a solution as a result of not having to assess the full detail of behaviour of all assemblies to the finest detail. The specification of capability will have a unique identifier that anchors the definition and allows it to be accessed. This reflects the same principle that gives rise to labels in a taxonomy, where the label recalls the abstract definition removing the need to understand the effect of the parts from first principles.

Today's solution at best have a coded form of the semantic interpretation that may not reflect the formal definition due to inaccuracies of interpretation. Many semantics are reduced to inconsistent labels that a user has to interpret. Whilst an LLM can do a reasonable job at interpretation of chaotic data, it will benefit a rigorous model traceable through formal definitions to fundamentals.

##Structural View: Occurrences as a Hyperedge Fabric

The component–system pattern above describes assembly informally: components have ports, and components combine into systems. This subsection gives that pattern a formal structural counterpart, without displacing the informal description above.

Every modelled semantic thing in this structural view is an occurrence. Structurally, an occurrence is hyperedge-like in that it can engage with multiple other hyperedges (unlike an edge which only has two places of engagement). The occurrence exposes ports that may join with compatible ports of other occurrences, and each joined pair of ports forms a point. The point is simply a recognition that two ports are joined. It is not itself a separate semantic node or vertex, and no meaning beyond “these two ports are joined” is smuggled into it. A port can only join with one other port. The port may have direction and role with respect to the internals of the occurrence. Where multiple ports are to be associated there is necessarily additional semantics requiring a further hyperedge occurrence where each port to be associated is joined with a single dedicated port on this new additional hyperedge.

The interconnected collection of occurrence-hyperedges is a hyperedge fabric: the structural projection, with occurrence semantics suppressed. When the semantics carried by those occurrence-hyperedges are exposed, the same fabric is a system. A system is therefore not a separate object layered on top of the fabric; it is the same fabric viewed with meaning restored. A selected system may itself participate externally as one occurrence at the next boundary — which is the formal counterpart of “a system can be represented as a component” in the component-system definition of Section 1.

This model does not require a primitive node or vertex category. A point is the incidence locus conceptually emergent by the joining of two ports, not an independently semantic vertex. Correspondingly, this document does not adopt classification-style typed nodes as a foundation.

##Semantic Relationships as Occurrences

A simple relationship, in the structural view of Section 5.1, is exhausted by port joining: two compatible ports meet (and form a conceptual point). As described in 5.1, anything more is semantic. A dependency, containment relation, adaptation, specification relation, flow relation, precedence relation, or other complex relationship among components, systems or specifications is therefore represented as an occurrence with its own ports. It then joins to the participating occurrences and becomes another hyperedge in the same fabric.

This removes the need for a special semantic-edge or relationship ontology separate from occurrence. An apparently binary or n-ary relationship among the components, ports and specifications already defined in Section 1 and Section 5 is simply an occurrence whose semantics state the participant roles and conditions, with one or more ports providing its structural incidence to those participants.

Applied to the terminology already introduced: the specification relation between a specification (Section 1) and the component or system it constrains is one such relationship-occurrence. Where that constraining relationship carries semantics of its own — for example, that it is a licensing constraint rather than a physical one — those semantics are stated in the occurrence representing the relationship (relationship-occurrence) itself, not left implicit in an unlabelled point.

#Specifications and LLMs
As discussed briefly above, LLMs can take advantage of specifications of capability. The LLM reasoning load can be reduced by working with the guaranteed behavioural abstraction provided in the specification for a component as opposed to working at the finest of details (it does not always need to understand the environment using string theory!).

The LLM can develop system solutions by assembling components of understood capability (using normal engineering and design processes) knowing that the behaviour of the components are internally controlled to be within the bounds of the specification. The LLM can then describe the behaviour of the system at its boundaries, i.e., of the component(s) that that system can realize. Hence the LLM can develop the specification for the components it produces.

For components not produced by a specific LLM (produced by another LLM or by a human), the LLM can assess the internal workings of the component (by reviewing the actual code/circuitry) at fine-grained detail. LLM reasoning can:

- extract the essential behaviour and abstract that to form a specification
- consider whether that abstracted behaviour defined in the specification appears beneficial in the formation of relevant systems and where not, propose simplifications
- evaluate the robustness of that essential behaviour and propose enhancements to ensure that the component operates within the desired bounds
- review existing specifications to determine whether other components already do a similar job
- etc.

The occurrence-semantics kernel introduced from Section 5.1 onward gives this reasoning a decidable substrate rather than a heuristic one. Where an LLM previously had to infer, from prose alone, whether two specification contributions were compatible, redundant or contradictory, it can instead evaluate merge over their opportunity regions directly: a non-empty intersection is a valid combination, an empty intersection is never, and an unconstrained property is void rather than silently absent. Ranking among remaining legal opportunities is likewise separated — as prefer rather than as an implicit tie-break — so that an LLM's proposed simplifications or enhancements can be checked against an explicit rule set instead of re-derived from context each time (Section 8a).


#Some specification examples
This section provides some simple examples and will reference the equipment capability draft and other future drafts.

##A temperature sensor
Consider a simple temperature sensor. The physical sensor will have an operational range, a precision, an accuracy, etc. It will provide output in particular units and may be able to indicate out of range. The sensor is itself a small system of components. It will be sensitive to power supply behaviour, humidity and other environmental factors.

All of the above will be included in the hardware specification of that physical component. That component when designed into a system will contribute to the system behavior.

For this example we will assume that the output for that sensor is available via a control solution and is presented at an externally accessible interface. We will assume that the presentation is in JSON and that presentation was defined in YANG.

In a the imagined application for this sensor, lets assume that the temperature is relevant only to whole degrees and is required to be in Celsius so an integer is used to represent the temperature.

With this level of coarseness the fine grained precision and accuracy of the actual component can probably be ignored (although the component may be pushed close to its limits and hence there may be an accuracy consideration etc.), but the operational range is potentially still relevant and environment effects that cannot be eliminated still need to be understood.

There may also be known failure modes that cause detectable incorrect readings that need to be accounted for.

So, considering the component alone, simply stating integer in the YANG model is not sufficient.

Going further, the temperature sensor has a particular role in the context of the equipment it is monitoring. There may be several temperature sensors on that single equipment. Traditionally they would have had distinct labels (although these were often potentially misleading). Whilst this may have been sufficient in a basic operations environment, much more can be done and is probably necessary current and future solutions.

Having an identifier is clearly necessary, but that should lead to an accurate and fully interpretable representation of the positioning of that component in the equipment in isolation and in the broader solution as a whole.

For example, the detector may be at the top of a circuit pack that is placed in an assembly with convection cooling where that detector is provided to measure the temperature of the airflow leaving the top of the circuit pack and hence feeding to the next equipment above.

For a full understanding of the implications of a measurement provided by that detector, a detailed understanding of its positioning and purpose is necessary. It is intended that the specification model provide such detail.

The specification model will be generalized such that the details provided can be used in any relevant application. It will not describe detailed per instance cases. Hence the specification will be used in conjunction with the actual instance arrangement to allow understanding of any reading in context.

Traditionally, with ad-hoc formatting and variable accuracy of definitions etc., only a well experienced SME would have a chance of determining the relevance of a detected value.

In a modern and future solutions we can do and have to do better. The intention is that the specification approach using the generalised specification definition structure set out in this document will provide a basis for LLM assisted specification generation and interpretation.


##Worked extension: a where rule and its projections

The operational-range consideration above can be stated as an explicit cross-occurrence rule rather than left as prose.

##Occurrence Semantics: Opportunity Regions and Merge

Sections 5 through 8 describe pruning, refactoring and specification informally, in terms of what a component or system can or cannot support. This section gives that description a formal, composable algebra, carried over from the occurrence-semantics kernel.

###Opportunity regions

An occurrence definition denotes the set, region or family of opportunities that satisfy it — its opportunity region, written [[A]] for occurrence A. A scalar exact value denotes a singleton region. A range, pattern, object shape, semantic relationship occurrence, fabric arrangement or explicit alternative denotes a wider region.

###Semantic states: void, valid, never

| State | Meaning | Ordinary merge behaviour |
| --- | --- | --- |
| void | No surviving semantic contribution for the property, relationship occurrence or rule occurrence at this stage. | Neutral: void merge X = X. |
| valid | A non-empty opportunity region or satisfiable semantic contribution/rule set. | Combines conjunctively with another valid contribution. |
| never | The accumulated hard semantic definition is unsatisfiable. | Absorbing until explicitly reset or pruned to void. |

###Conjunctive merge

Merge accumulates obligations. All surviving hard contributions must hold. Ordinary merge is intended to be commutative, associative and idempotent after explicitly ordered transformations have been applied.

###Alternatives, ranges, negation and exclusions

Alternatives must be explicit. Ranges and patterns define opportunity regions; exclusions preserve the original positive statement while removing illegal islands.

#Recursive pruning and refactoring
This builds on the example sketches and formalizes the process of recursive pruning and refactoring.

The essential process involves defining a general abstracted thing at some intermediate point in the progression of refinement (e.g., a temperature sensor), setting out a reasonable derivation path from the most generalized component and then refining that general abstract thing by recursive pruning and refactoring to arrive at the necessary specialization.

The following subsections take some generalised cases to illustrate the process.

##Thing to component
In this approach a thing has all possible functions and capabilities of anything imaginable. Moving to component via pruning and refactoring involves recognition of the concept of boundary of a thing and then facet of a boundary, i.e., a surface that can "interface" with the surface of another thing. From facet, we can derive port which is a specific place on the surface where an interface can be formed. The idea of port is fundmental in the essence of a component as it is the place where the component capabilities are accessed.

The same essential approach can be used to move from assembly of things being a thing to the more formalised component system pattern.

A component can be physical or abstract functional. All components have some active influence on their environment (unlike a specification which is an informational thing and is inherently passive). The generalized abstract functional component is a pruned form of the generalized component. It includes all possible behaviours. It is still too general to apply meaningfully and requires further pruning.

##Component to specific termination point
A termination point as per [RFC8345] is a specific pruned functional component that offers at its ports a defined subset of all possible functions. It does not offer the capability to forward information over great distances but does offer the ability to provide access to a flow of information at a specific place. In other standards [ITU-T G.7711] the LogicalTerminationPoint has roles including in one direction processing an incoming flow determining timing and framing and extracting the content "payload".

The termination point is still general and requires refinement to represent what is really feasible and useful in a network deployment context.

Up to this point refinement was carried out via pruning and refactoring where each level resulted in an explicit relabelling Thing -> Component -> TerminationPoint. Traditionally, the same orientation of progression was considered as a process of classification where properties were added as opposed to removed and the process continued beyond this point to highly specialised classes.

In the approach realized via [RFC8345] and [ITU-T G.7711], further refinement is carried out by augmentation. Here augmentation essentially exposes properties that were already encompassed by the definition of the thing being augmented. It is not an extension, it is an exposure of underlying properties.

So a termination point that processes photons is represented via an augmentation of the generalised termination point. Likewise, the termination point that process Ethernet is represented via an equivalent augmentation. Clearly, an augmentation of a termination point with photonic and Ethernet properties is not rational.

This is where the specification becomes critical. Each specific realization of a termination point type in software or hardware will be distinct. Just because it is an Ethernet termination point type does not mean it is the same as all other Ethernet termination point types. Of course, there will be many many instances of the type and they will have identical functional capability.

Setting out the distinct capabilities of the type is the role of the specification. The specification will be constructed by assembling pruned and refactored specifications of more complete definitions. So, for example, the Ethernet standard may define MEP and MIP capabilities, but the type of termination point may only support MEPs and there may be 7 levels of MEP in the standard, but this termination point type may only support 1 level where the measurements available to the MEP may be limited and a specific measurement constrained in range. All of this detail is available via the specification.

Armed with the specification a controller can determine precisely how the termination point can be applied in a solution and the range of opportunities available.

Whilst designing a solution, the controller may use a specific type of termination in a restricted form. For example, the Ethernet termination, although capable of supporting a MEP may be required to not provide that capability. The design of the pattern of use of terminations in a system may utilise the same type several times in the pattern where each occurrence in the pattern has a distinct further narrowing of the capability of the type. This is discussed further in Specification of an assembly (add reference to section).

Eventually the pattern will be realized in a network. This will first be designed with no real instances in place. This will be represented with further specific narrowed termination point occurrences. Finally, there will be real instances in the network. These can also be considered as occurrences.

The word “pruning” has been used informally throughout this section for two related but distinct operations, which Section 9.5 below separates formally: removing a source contribution before it is combined with others (source pruning), and removing a property from the already-combined result (result pruning). The MEP-level restriction described above is an instance of the latter: the full MEP capability survives assembly from the Ethernet standard's specification, and is only then narrowed to one supported level at this termination point type.

##Further examples
-Thing to Component to physical thing to equipment to specific equipment type to use of that equipment to instance of equipment
-A plug example
Circle back and relate this more rigorous section to the specification examples.

##Cross-Occurrence Rules, Preferences and Contradiction

The account of assembling and narrowing termination points refers informally to constraints that span more than one occurrence.

###where: hard cross-occurrence constraints

Property-local opportunity spaces are not sufficient for semantics that span several members of a fabric. A where block states hard conditions over a selected semantic region within or across occurrences. Inter-property constraints are the common local case: the relevant property occurrences participate in the same applicable fabric, and the constraining relationship is represented semantically by a rule occurrence.

###prefer: ranking without elimination

A prefer block orders or optimises legal opportunities. Conflicting preferences produce a trade-off, unresolved ordering or profile-specific optimisation problem rather than never.

###Regional contradiction and localisation

A contradiction may arise only when several occurrence-hyperedges and rule occurrences are considered together. In that case the applicable sub-fabric or semantic region, rather than any one member, initially becomes never. Every constituent occurrence may remain locally satisfiable even though no legal semantic combination exists for the fabric as assembled.

#Specification of an assembly
Build on the examples and the recursive pruning and refactoring to explain the subtle narrowings in a system/scheme spec. Describe the essential process.
Use examples to illustrate the progression:
- Same examples as recursive pruning and refactoring but focus on role and subtle specializations in role
List other examples.

Section 8.2 showed that a single termination point type may be used several times within one system-level pattern, with each occurrence in the pattern carrying a distinct further narrowing of the type's capability — for example, one Ethernet termination point in a pattern supporting a MEP and another, of the identical underlying type, restricted so that it does not. This section further develops towards a formalisation of how a system specification records those per-occurrence narrowings without treating each restricted use as a new type.

##An assembly as a fabric of narrowed occurrences

A system spec, as introduced in Section 5, is a pattern assembly of subtly specialized occurrences at a particular level of specialization, arranged in a meaningful structure that yields a relevant behaviour. In the structural terms of Section 5, that assembly is a hyperedge fabric: each participating occurrence — here, each use of the Ethernet termination point type — is a hyperedge exposing ports, and the arrangement of the pattern is expressed through ports that join to their neighbours in the assembly.

Two occurrences in the same assembly may derive from the same underlying specification and yet carry different opportunity regions, because each occurrence's local definition merges the shared specification with a source contribution specific to its role in the pattern.

##Role and subtle specialization within an assembly

A narrowing within an assembly is frequently a matter of role rather than of physical capability: two tps may be supported by identical hardware, differing only in the role the pattern assigns to each occurrence. Section 4's Role dimension (desired, specified, committed, claimed, observed, realised) and Section 10a's role-qualified exactness apply directly here — an occurrence's narrowing may be a specified restriction (a design-time decision that this occurrence will not offer MEP support) well before it is a committed or realised one.

##Cross-occurrence constraints within the assembly

Some narrowings cannot be expressed occurrence-by-occurrence at all, because the constraint spans several members of the pattern. Section 8.4.1's where rules apply at the level of the assembly's fabric, not only within one occurrence's local definition.

##Progression from designed pattern to realised network

Section 8.2 traced a pattern from design-time (no real instances in place) through further narrowing to real network instances, noting that instances are themselves occurrences rather than a terminal category. A fully precise, deployed occurrence of AssemblyPattern may acquire further definition within a subordinate control system etc.

##Active Pruning and the Refinement/Pruning/Refactoring/Mapping Distinction

Section 8.2 noted informally that “pruning” has covered more than one operation in this document. This section separates those operations formally and relates each to the terminology already defined in Section 1.

###void as an active operation

void (Section 7a.2) has an active transformational use as well as denoting neutral absence. Applying void at a definition stage removes a selected source contribution or resets an accumulated property occurrence, rule occurrence or semantic relationship occurrence to void.

###Source pruning versus result pruning

The distinction anticipated in Section 8.2 is between removing a contribution before it is combined with others, and removing a property from the already-combined result:

###Refinement, pruning, refactoring and mapping as distinct operations

Refinement is the broad progression by which an occurrence definition is developed. It may involve conjunctive merge, additional constraints, explicit alternatives, pruning, mapping or structural transformation.

- Refinement: the umbrella progression by which an occurrence becomes differently or more fully defined.
- Pruning: an explicitly scoped removal or reset of a property occurrence, semantic relationship occurrence, rule occurrence or source contribution.
- Refactoring: a transformation from one semantic hyperedge fabric to another that preserves declared correspondence rather than silently changing meaning; it often compresses a detailed component-system fabric at one level into a more compact exposed fabric at another.
- Mapping: a semantic hyperedge fabric, and hence a system, whose mapping occurrences record correspondence between source and target fabrics, including identity, derivation, value or structural transformation, direction, fidelity, reversibility and any information loss.

#Generalization of the specification
Build a specification structure from the examples and show the references and reuses.
Explain how the specification relates to the things in the problem space.
Lay out the specification structure.

The temperature-sensor and termination-point examples of Sections 7 and 8 illustrate individual specifications. This section generalises from those examples to the structure of a specification as such, and relates that structure back to the five conflated concerns identified in Section 3: generic definition, capability definition, policy definition, system combination, and operational instance.

##A specification as a bounded reference to be reused

A specification, as defined in Section 1 and elaborated in Section 5, describes an occurrence's capabilities in terms of occurrences achieved via similar pruning: it is not a flat, self-contained description but a reference structure that other specifications, and other occurrences, can merge, refine or address by identifier. The unique identifier requirement already stated in Section 5 is what makes this reuse practical rather than merely conceptual.

The EthernetTP specification used in Section 9 is a direct illustration: it is defined once, referenced by qualified identity (Section 11a) from every occurrence that uses it, and each occurrence narrows it independently. This is the general pattern the temperature-sensor example anticipates informally in Section 7 (“the specification will be used in conjunction with the actual instance arrangement”) and which Section 9 formalises for assemblies.

##Reference and reuse: relating specifications to each other

A specification may refer to one or more other specifications, as already stated in Section 5. Two distinct forms of reference recur across the examples in this document:

- Compositional reference, where a specification's structure includes properties specified elsewhere.
- Constraining reference, where a specification narrows another by merge rather than by including it structurally.

Both forms use the same underlying merge algebra (Section 7a); they differ only in whether the referring specification's own properties are new (compositional) or a narrowing of the referenced specification's existing opportunity region (constraining).

##Relating the specification structure to the five conflated concerns

Laid out this way, the specification structure maps directly onto the five concerns of Section 3:

- Generic definition: The most general, least-pruned specification in a reference chain — e.g., the universal component.
- Capability definition: A named, identifiable specification (such as EthernetTP) capturing what a type of component or system can support, independent of any particular deployment.
- Policy definition: A constraining reference (Section 10.2) applied at a particular occurrence to remove capabilities the operator does not want offered, expressed as a merge with a policy-sourced contribution rather than as a separate, informal restriction.
- System combination: An assembly specification (Section 9) in which the same underlying specification is referenced by multiple occurrences, each independently narrowed for its role in the pattern.
- Operational instance: A fully or highly precise occurrence (Section 10.4) reached by continued definition (Section 17b) from the specification structure above it, remaining subject to the same merge and where semantics rather than becoming a terminal, unrelated object.

Read this way, the specification structure is not a sixth, separate artifact alongside the five concerns of Section 3 — it is the single structure within which all five are positioned as different points along one reference-and-refinement chain, addressed by qualified identity and combined by the same merge algebra throughout.

##Completeness, Exactness and Role-Qualified Identity

Section 10 generalised the specification structure and related it to the five conflated concerns of Section 3, including operational instance as a fully or highly precise occurrence. This section makes precise what “fully or highly precise” means, and why exactness alone does not resolve the ambiguity between what a model element is and what it can support that Section 3 identifies as a core problem.

###complete and omission

By default, omission makes no local contribution: whatever survives from the sources remains defined. complete changes omission semantics at the stated structural boundary — properties not stated locally are actively pruned to void.

Complete is local, not terminal. It closes the visible property set at one boundary; it does not prohibit later composition, new relevant properties at another viewpoint, subordinate structure, or finer explanation — consistent with Section 17b's continued-definition principle below.

###Exactness, completeness and concreteness are independent

| Concept | Meaning |
| --- | --- |
| Exact property | Only one visible opportunity remains for that property. Exactness alone does not state its role or guarantee real-world satisfaction. |
| Complete boundary | Unstated inherited properties are pruned at this boundary. |
| Concrete occurrence | Sufficiently constrained for the current purpose or interaction. |

None implies either of the others. A provisioned connection may have exact endpoints and still leave route, equipment tuning and observed state open for later definition — the same pattern the network-instance discussion in Section 8.2 and the assembly-progression discussion in Section 9.4 both rely on without previously naming it.

This is the formal counterpart of Section 5's existing intent discussion where even the tightest constraint of a single value is essentially a statement of intent as it is impossible to guarantee that a property will be set, and of the Role dimension added to Section 4. Role-qualified exactness resolves the apparent tension between the two: an occurrence can be exact and still be only desired, or only specified, without contradiction, because exactness and role are independent axes.

#Characteristics of a language of specification
The language needs inherent capabilities (as opposed to after the fact bolt-on warts)
Extract key characteristics from above and from mobo
- narrowing requires specific redefine (relate to pruning)
- occurrence is an assembly of constrained type and specific values
- need to reference other specs as reusable parts
- refactoring, minor specialization and assembly
- interrelationship and influence
- uncertainty and preferences
(Need to review mobo and TR-547 spec, component-system etc.)

The occurrence-semantics kernel introduced from Section 7 onward satisfies each of these six characteristics directly, rather than requiring a further after-the-fact addition.

##Namespace-Qualified Identity

The reference-and-reuse discussion in Section 10.2 depends on being able to say when two contributions — arriving through different refinement or composition routes — are contributions to the same property, relationship or rule. This section makes that identity criterion explicit.

Automatic semantic combination occurs only when contributions resolve to the same qualified identity. Property occurrences, semantic relationship occurrences and rule occurrences should use a stable namespace plus local name; a prefix is only a local alias.

Contributions may arrive through different and arbitrarily deep occurrence routes and still combine when their qualified identity is the same — consistent with this document's existing position (Section 5, Appendix A.1) that there is no single canonical refinement path. There need not be one canonical derivation path or one privileged parent. Revision participates in source resolution and compatibility; whether it forms part of permanent identity remains a separate versioning decision.

#Specification language options
Landscape of languages... does anything do this?
Take YANG and enhance (as discussed in mobo)

The characteristics enumerated in Section 11 rule out a bare extension of YANG or JSON Schema on their own: neither has a native notion of void versus never (Section 7), neither separates hard constraints from ranked preferences (Section 8), and neither has an explicit merge algebra usable across independently sourced contributions (Section 7).

Positioned against existing language families:

- YANG / JSON Schema — close to this document's surface notation and directly reachable as a projection (Section 17), but without void/never, merge, where/prefer or qualified cross-source identity as native constructs.
- UML (class and metamodel diagrams) — reachable as a projection of source merge and recursive constraining occurrences (Section 17), useful for presentation but not as an evaluation semantics.
- Constraint/configuration languages in the general family of CUE, Nickel and Alloy — each already provides one part of this document's algebra (conjunctive unification, explicit priority/overlay, or relational bounded search respectively) but not the combination of merge, void/never, where/prefer and qualified identity as a single kernel.

No existing language landscape covers this combination as a single kernel.

#Building a specification structure
Tooling and support to build and interrelate.
Catalogue/library of specs
Deep application... machine interpretable structure in all standards
Use of AI to reverse engineer specs with guidance... peer review and testing cycle

Building and maintaining a catalogue of specifications under this framework requires processing to happen in a defined order, so that tooling built independently by different implementers produces the same result from the same sources. The following sequence is normative for that purpose:

1. Resolve module, namespace, revision and qualified-name identity (Section 11).
2. Evaluate each source occurrence and its exposed semantic hyperedge fabric recursively without discarding contribution provenance (Section 16).
3. Apply source-scoped transformations such as prune and active void (Section 9).
4. Reconcile the applicable port/point incidence, then merge surviving occurrence, property, semantic relationship and rule contributions conjunctively within the applicable semantic fabric (Section 7).
5. Apply the local body: constraints, explicit alternatives, exclusions and local void (Section 7).
6. If complete, prune inherited properties and associated contributions not stated at that structural boundary (Section 10).
7. Bind and evaluate surviving hard where rule occurrences over their selected semantic hyperedge fabrics and resulting opportunity regions (Section 8).
8. Normalise property and regional opportunity constraints; record property-level or regional never (Section 8).
9. Combine surviving prefer rules to rank legal opportunities using the selected preference profile (Section 8).
10. Retain explanation records for surviving, pruned, contradictory and preference contributions (Section 16).

A rule that survives while referencing a void or unavailable property needs an explicit policy. The basic profile should require existence guards for optional paths; an unguarded required reference that cannot bind should normally make the applicable rule unsatisfied rather than disappear silently.

#A specification evolution example
Discuss how a spec may change as understanding emerges and how it may be refactored.

Continuing the EthernetTP example from Section 9: suppose the Ethernet standard referenced by EthernetTP is revised to add a new MEP-adjacent capability, and separately, operational experience shows that the existing mepLevel range was too permissive for the service constraint. Both changes can be expressed as ordinary evolution of the specification's opportunity region, without discarding or silently overriding the occurrences already merged from it.

Where the evolution is better understood as a refactoring — for example, splitting EthernetTP into a base termination-point specification and a separate MEP-capability specification referenced from it this is refactoring rather than pruning, and the correspondence between the old, single specification and the new, split pair should be recorded as an explicit mapping rather than left to be inferred from the fact that both specifications happen to share a name.

#A system specification example
Take the language considerations and set out system specs in a more formal way

This section will set out in a compact surface notation that is to be developed in a later draft.

#Broader Application of the Language
Negotiation
Refinement of planning
Development of standards
Expression of uncertainty and pattern

The four applications named in the original outline map onto constructs already introduced in this document, rather than requiring separate treatment:

Negotiation: The process of formulating intent through negotiation and resultant gradual refinement has a similar feel to the degrees of pruning and refactoring of the specification. Formally, a negotiation position is an occurrence; a counter-position narrows or merges it; and where two positions' opportunity regions have empty intersection, the negotiation is at an explicit, detectable impasse (never) rather than an ambiguous stalemate. Prefer expresses a party's ranked preferences among positions that remain jointly legal.

Refinement of planning: The assembly-progression — design-time pattern through commitment to realised network instance — is itself a planning refinement expressed entirely through merge, role and continued definition, without a separate planning-specific mechanism.

Development of standards: Evolution, discussed earlier, is the process a standards body uses to revies a specification. Existing conformant occurrences remain valid by construction under refines. Refactoring/mapping distinction covers the harder case of a standard being restructured rather than merely narrowed.

Expression of uncertainty and pattern: Ranges, alternatives and exclusions express uncertainty in the value itself; role-qualified exactness expresses uncertainty in the status of an otherwise-exact value (desired versus committed versus observed, for instance); and prefer expresses uncertainty in which of several legal opportunities is wanted, as distinct from which is permitted.

##Rules as Occurrences and Provenance

This section will provide an expanded form and states the provenance requirement referenced earlier in the draft.

#Compact surface form

The examples in this document will be written in a compact form. This section will discuss that form and will project possible language equivalents.

##Projection Equivalences

| Projection | Occurrence interpretation |
| --- | --- |
| Concrete JSON | A chosen complete view whose visible values are precise. |
| JSON Schema / YANG | A chosen complete view whose values are constraints and whose structure states permitted properties. |
| UML inheritance | A diagrammatic projection of source merge and cumulative refinement. |
| UML metamodel | Recursive constraining occurrences shown through conventional metalevel notation. |
| CUE-like | Conjunctive unification, explicit disjunction and contradiction. |
| Nickel-like | Programmable generation and explicit priority/overlay profiles. |
| Alloy-like | Relational constraints and bounded search over a chosen occurrence context. |
| Component–system | The basic structural/semantic view: component-role occurrence-hyperedges expose ports; compatible port pairs join to form points; with occurrence semantics exposed the resulting fabric is a system, which may itself participate as one component across its boundary. |
| Structural refactoring | A transformation from one semantic hyperedge fabric/system to another, often presenting a detailed lower-level fabric as a more compact target fabric through an explicit mapping fabric. |
| Hyperedge fabric | Structural view: occurrence-hyperedges joined through port-joining points, without a primitive node/vertex ontology. |
| System | Semantic view of that same fabric, with meaning exposed in each occurrence-hyperedge. |
| Mapping system | A semantic hyperedge fabric whose mapping occurrences connect source and target fabrics and carry correspondence, transformation and fidelity semantics. |

The target is loose semantic and structural equivalence. These projections do not import terminal instances, permanent metalevels or source-language evaluator identity into the kernel.

#Continued Definition Below a Precise Occurrence

A fully precise, deployed occurrence remains open to further definition rather than becoming a terminal instance. This section will state that principle formally.

#Conclusion
Mindset Change
Language challenges
Use of AI
Target is an ecosystem of specs driving agentic components...

The occurrence-semantics kernel discussed in this revision will give that ecosystem a common, composable substrate: every specification, component, relationship, rule and mapping in it is expressible as an occurrence with a well-defined opportunity region, combined by one merge algebra, and checkable by one normative processing sequence. This does not negate the mindset change already noted above — rigorous modelling still requires giving up ad hoc, source-order-dependent combination in favour of explicit, provenance-preserving definition — but it gives that change a formal target to converge on, consistent with the historical-lineage position that this kernel formalises, but does not retrospectively attribute to, the standards and drafts it builds on [ITU-T_G.7711] [ONF_TR-512] [mobo].

#Security Considerations

TBD

An implementation that silently localises a regional never rather than propagating it can present a capability, policy or licensing structure as valid when it is not. This has security as well as correctness implications.

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



##A.5. Structure and Semantics Are Separable

A.1 through A.4 above describe emergence and refinement without needing to separate structure from semantics explicitly. Section 5 makes that separation formal: the same occurrence fabric has a structural projection (occurrence-hyperedges joined at ports, with semantics suppressed) and a semantic projection (the system, with occurrence semantics exposed).

#Acknowledgments

This document has been made with consensus and contributions coming from multiple drafts with different visions. We would like to thank all the participants in the IETF meeting discussions.

{:numbered="false"}
