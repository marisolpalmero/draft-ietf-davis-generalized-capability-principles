---
title: "Equipment Capability Application"
abbrev: "EquCapAppl"
docname: draft-davis-ivy-equipment-capability-application
category: info
stand_alone: true

submissiontype: IETF  # also: "independent", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
wg: "IVY WG"
keyword:
 - capability
 - manifest
 - specification
 - representation
 - occurrence
 - component-system
 - pruning&refactoring
venue:
  group: "Network Inventory YANG WG"
  type: Working Group
  mail: "inventory-yang@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/inventory-yang/"
  github: "marisolpalmero/draft-ietf-davis-generalized-capability-principles"
  latest: "https://github.com/marisolpalmero/draft-ietf-davis-generalized-capability-principles/blob/main/draft-davis-ivy-equipment-capability-application-latest.md"

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

   GenCapPrin:
              title:  "Generalized Capability Principles"
              target: https://github.com/OpenNetworkingFoundation/TAPI/releases/tag/v2.4.0

--- abstract

This document applies the generalized capability principles to the description of physical equipment and shows how such capability specifications integrate with base inventory and entitlement models as defined in [BaseInventory] and [EntitlementInventory].

The approcah is expmained by example, focusing on how the potential capabilities of equipment types are described, how these map to entitlements (licensed or policy-controlled subsets of capabilities), and how they are instantiated as inventory items.  The explanation covers both physical capabilities and emergent functionality.

--- middle

#Terminology
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT"
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in the
document are to be interpreted as described in RFC2119}}.

The following terms abbreviations are used in this document:

* equipment: A physical item necessary for a particular purpose.
* physical: Has spatial dimensions (i.e., can be measured with a "ruler") and in some cases has mass (i.e., can be weighed with scales)
* SFP: Small Form-factor Pluggable


#Introduction
Physical items have various fundamental properties such as length, temperature, weight. In an assembly of phsyical things each thing plays various roles in the structure and has to be compatible with the other things in that structure so that it can participate in those roles.

In a telecoms environment, there are many physical things that support the provision of service. For simplicity, in this document a physical thing that is useful for the provision of telecommunication service will be refered to as an equipment. The focus of this document is limited to telecommunications networks and hence equipments for related purposes, but there is no specific limitation to the method that prevent it from being applied more broadly. This restriction is simply to reduce the volume and complexity of the descriptions.

The equipments to be represented include boards (circuit packs) and shelves (subracks). In this description an SFP will be considered as a board. The essential structural model is that a shelf can be placed in a rack, a board in a slot in a shelf and a board (SFP) in a slot in a board.

Whilst this general model says a board can be placed in a slot, clearly not all boards can be placed in all slots. This document describes the opportunities in terms of physical capabilities of types of physical things where a type of thing is a set of tings having common characteristics such that all relevant characteristics are the same such that the things can be interchangeable.

An inserted board supports functionality. Some functions emerge from a combination of boards. The specification method described here allows the functions that emerge from assemblies of equipment to be described in detail. In some cases the emergent capability requires power to be applied (the capability to amplify a signal, the capability to decode a bit stream), in other cases the capability is inherent in the thing itself (the capability to physically support something else, the capability to convey light in a channel).

A majority of powered physical things can be configured and can have their behaviour define in code and data. The capability, in these cases, is emergent from the combination of the physical structure activated by power and shaped by data and code. Hence the overall capability specification may be defined by a complex combination of capability specifications.

The specification of potential emergemt functions can be used at various stagest of the network lifecycle. 

#Problem Statement
A telecoms network is realized through an assembly of equipments (such as curcuit packs, boards, racks, cables etc.), some passive (not directly powered), some active (directly powered) and some running complex software etc. Each assembly provides some capability that supports the provision of service. Understanding these capabilities in detail and precisely is vital throughout the life of the network.

Whilst an active equipment may provide an interface that exposes what is available currently, it rarely indicates what is potentially avaliable and when it does this is usually through an ad-hoc mechanism which only conveys a limited view of capability. Clearly, when the equipment is not powered, it is not possible to interrogate it even for this sparse and basic information. Passive equipments cannot be interrogated.

Manufactures of equipments produce types of equipment where each instance of that type is essentially identical with respect to capabilities within an acceptable and understood tolerance. Any instance of a type of equipment is interchangeable with another instance of the same type. There may also be other compatibilities where different types have the same or a superset capability and hence can be used as alternatives.

It is necessary to understand some aspects of capability of a type of equipment at all stages of the lifecycle:
- whilst speculation about services to be provided prior to network design and researching potential network capabilities
- when planning network structure
- prior to purchasing, when choosing particular equipment types for a specific purpose where there are alternatives
- when planning future deployment of equipments
- when the equipment is installed and services are being designed
- even when the equipment is fully configured and operational with no errors etc., there may be heartbeat and status capabilities

Considering the above, it is necessary to have a complete description of capability that is available independent of the presence of equipment. This description needs to be rigorous and readily interpretable allowing for comparisons with other equipment types etc. On that basis the capabilities should be described in a normalized language where advantage is taken of recurring patterns etc.

As automation progresses, machine interpretability of the capability information becomes increasingly important. Whilst AI, especially LLMs, can deal with the variety of human interaction, a more efficient and compact language usage is preferable for efficiency and removal of potential ambiguity.

This document sets out an approach for expression of capabilities of equipment in terms of physical structure, software/data structure and emergent functionality.

Whilst knowing the YANG model for the equipment is beneficial, it is not sufficient. The YANG model essentially provides a space within which actual state and configuration can be expresses. The YANG model tends to not express equipment type based constraints. Whilst specifying the commbinatorial effects of interacting equipments and software in YANG is potentially possible, the mechanisms availble are not designed for this purpose and the results would probably be a large set of special models with extremely cumbersome/complex definitions that would be distinct from the interface model that is necessarily open and broad.

General modeling framework use

#Specification in terms of the Model

The specification of equipment capability should be presented in terms of the *generalized capability model* from [GenCapPrin] and explicitly mapped to the inventory and entitlement contexts.

The relationships between these elements can be summarized as:

| Concept        | Defined In               | Represents |
|----------------|--------------------------|-------------|
| Capability Spec| [GenCapPrin], this draft | The potential functions and limits of an equipment type |
| Entitlement    | [EntitlementInventory]   | The subset of capabilities permitted or licensed |
| Inventory Item | [BaseInventory]          | The actual occurrence of an entitled capability in the network |

This linkage ensures that refinement and occurrence formation have a tangible operational anchor in network management systems.



#Some specification examples
This section should describe process illustrated via example sketches and detail.

Note that the use of the physical and functional considerations are recursively intertwined... a bit of physical, emergent function, function assembly, physical assembly thermals etc.

Start with a generalized equipment and a generalized function and sketch the narrowing.

A physical equipment in a physical context considering how it fits and what fits in it...
-type
-size
-thermals
-power/thermal
-physical compatibility
-electrical compatibility
-diagram or compatibilities from ONF work
-physical assemblies and thermals

Function emergent from a physical equipment
-raw functions (use ProcessingConstruct recursion as per ONF)
-emergent capabilities and needs
-functional compatibility
-power and thermals per functions

A system of equipments in an assembly
-functional assemblies
-power and thermals per assembly

A system arrangements for a protection scheme.

A specification for a system arrangement for a service and associated realization pattern specifications.

#Recursive narrowing
This general principle is considered in the context of equipment specification.

#Specification of an assembly
Highlighting this general principle in terms of assemblies of physical things and the emergent behaviour that results.

#Generalization of the specification
Showing the reuse of specicifation fragments.

#Using the language of specification
This requires work in the generalized capabilities draft.

#Building the equipment specification structure
Take the language  and general structure and build specific equipment.

#An equipement system specification example
Take the language considerations and set out system specs in a more formal way

#Conclusion
Highlight the stage of the eork.

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
