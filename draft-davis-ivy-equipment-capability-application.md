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

   EntitlementInventory: I-D.ietf-ivy-entitlement-inventory

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

This document applies the generalized capability principles to the description of equipment (a physical thing) with applied data (configuration state and code (software, firmware etc.)) and shows how such capability specifications integrate with base inventory and entitlement models as defined in {{BaseInventory}} and {{EntitlementInventory}}.

The approach is examined by example, focusing on how the potential capabilities of each equipment type-version with applied data are described, how these map to entitlements (licensed or policy-controlled subsets of capabilities), and how they are instantiated as inventory items.  The explanation covers both the capabilities of equipment in terms of physical properties and the capabilities of equipment with applied data in terms of resultant emergent functionality.

--- middle

#Terminology
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT"
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in the
document are to be interpreted as described in RFC2119}}.

The following terms abbreviations are used in this document:

* equipment: A physical item necessary for a particular purpose.
* physical: Has spatial dimensions (i.e., can be measured with a "ruler") and in some cases has mass (i.e., can be weighed with scales)
* SFP: Small Form-factor Pluggable
* equipment with applied data: A physical item with compatible software, firmware, configuration etc.
* equipment type-version: A reference to a definition of the capabilities of an equipment such that all instances of equipment of that type-version have the same capabilities.

#Introduction
Physical things have various fundamental properties such as length, temperature, weight. In an assembly of physical things each thing plays various roles in the structure and has to be compatible with the other things in that structure so that it can participate in those roles.

In a telecoms environment, there are many physical things that support the provision of service. For simplicity, in this document a physical thing that is useful for the provision of telecommunication service will be referred to as an equipment. The focus of this document is limited to telecommunications networks and hence equipments for related purposes, but there is no specific limitation to the method that prevent it from being applied more broadly. This restriction is simply to reduce the volume and complexity of the descriptions.

The equipments to be represented include boards (circuit packs) and shelves (subracks). In this description an SFP will be considered as a board. The essential structural model is that a shelf can be placed in a rack, a board in a slot in a shelf and a board (SFP) in a slot in a board.

Whilst this general model says a board can be placed in a slot, clearly not all boards can be placed in all slots. This document describes the opportunities in terms of physical capabilities of an equipment type-version where all relevant characteristics are the same for each instance of that type-version such that the instances can be interchangeable.

Many equipments can accommodate applied data. The desired functionality is emergent from the combination of that equipment with applied data. The statement of capability covers the equipment with applied data.

This document is part of a suite that includes:

* {{GenCapPrin}} defines the generalized capability and refinement principles.
* {{BaseInventory}} defines how equipment occurrences are represented in a network inventory.
* {{EntitlementInventory}} defines how capability entitlements and licensed functionality are tracked.

Together, these drafts describe a continuous trace:

Capability -> Entitlement -> Inventory -> Realization


~~~ aasvg
{::include art/capabilities.txt}
~~~
{: #fig-capabilities title="Relationship between Capability, Entitlement, and Inventory" }


where:

*  **Capability** defines what an equipment with applied data *can* do (its potential);
*  **Entitlement** defines what an operator *is allowed* or *enabled* to use; and
*  **Inventory** records what *actually exists* and is deployed.

The goal of this draft is to show, by concrete examples, how the generalized capability framework is specialized for equipment with applied data and how that structure integrates with inventory and entitlement data.

From a purely physical perspective, whilst the general model says a board can be placed in a slot, clearly not all boards can be placed in all slots. This document describes the opportunities in terms of physical capabilities of types of equipment.

A majority of equipment can be configured and can have its behaviour define by software etc. The capability, in these cases, is emergent from the combination of the physical structure activated by power and shaped by data. Hence the overall capability specification may be defined by a complex combination of capability specifications.

An equipment supports some specific functionality. Some functions emerge from a combination of equipment. The specification method described here allows the functions that emerge from assemblies of equipment to be described in detail.

The specification of potential emergent functions can be used at various stages of the network lifecycle. The specification of emergent functions allows a purchasing application to determine which particular types of equipment can be acquired and a planning tool to determine how to arrange occurrences of types of equipment in systems and what data to apply to support particular functions prior to the purchase of any equipment.

#Problem Statement
A telecoms network is realized through an assembly of equipments (such as circuit packs, boards, racks, cables etc.), some passive (not directly powered), some active (directly powered) and some running complex software etc. Each assembly provides some capability that supports the provision of service. Understanding these capabilities in detail and precisely is vital throughout the life of the network.

Whilst an active equipment with applied data may provide an interface that exposes what is available currently, it rarely indicates what is potentially available and when it does this is usually through an ad-hoc mechanism which only conveys a limited view of capability. Clearly, when the equipment is not powered, it is not possible to interrogate it even for this sparse and basic information. Passive equipments cannot be interrogated.

Manufactures of equipment produce instances of types of equipment where each instance of a type is essentially identical with respect to capabilities within an acceptable and understood tolerance. Any instance of a type of equipment is interchangeable with another instance of the same type. There may also be other compatibilities where different types have the same or a superset capability and hence can be used as alternatives.

In practice, managing equipment capabilities in isolation is insufficient.  Each capability must be tied to:

*  an *entitlement* indicating whether it is licensed or permitted for use, and
*  an *inventory record* that anchors the capability to a deployed occurrence.

This tri-layer relationship enables operators to reason about what equipment types exist, what functions they can theoretically perform, what data needs to be applied to cause these functions to be performed, what has been purchased or activated, and what is currently deployed or configured.

Without such linkage, automation frameworks cannot determine whether a planned configuration is feasible, legally licensed, or available in the installed base.

It is necessary to understand some aspects of capability of a type of equipment with applied data at all stages of the lifecycle:
- whilst speculation about services to be provided prior to network design and researching potential network capabilities
- when planning network structure
- prior to purchasing, when choosing particular equipment types, software etc. for a specific purpose where there are alternatives
- when planning future deployment of equipments, software etc.
- when the equipment is installed and services are being designed
- even when the equipment is fully configured and operational with no errors etc., there may be heartbeat and status capabilities

Considering the above, it is necessary to have a complete description of capability that is available independent of the presence of equipment etc. This description needs to be rigorous and readily interpretable allowing for comparisons with other equipment types etc. On that basis the capabilities should be described in a normalized language where advantage is taken of recurring patterns etc.

As automation progresses, machine interpretability of the capability information becomes increasingly important. Whilst AI, especially LLMs, can deal with the variety of human interaction, a more coherent and compact language usage is preferable for efficiency and removal of potential ambiguity.

This document sets out an approach for expression of capabilities of equipment in terms of physical structure, data structure (software etc.) and emergent functionality.

Whilst knowing the YANG model for the equipment is beneficial, it is not sufficient. The YANG model essentially provides a space within which actual state and configuration can be expresses. The YANG model tends to not express equipment type based constraints. Whilst specifying the combinatorial effects of interacting equipments and software in YANG is potentially possible, the mechanisms available are not designed for this purpose and the results would probably be a large set of special models with extremely cumbersome/complex definitions that would be distinct from the interface model that is necessarily open and broad.

#Specification in terms of the Model

The specification of capability of equipment with applied data should be presented in terms of the *generalized capability model* from {{GenCapPrin}} and explicitly mapped to the inventory and entitlement contexts.

The relationships between these elements can be summarized as:

| Concept        | Defined In               | Represents |
|----------------|--------------------------|-------------|
| Capability Spec| {{GenCapPrin}}, this draft | The potential functions and limits of an equipment type |
| Entitlement    | {{EntitlementInventory}} | The subset of capabilities permitted or licensed |
| Inventory Item | {{BaseInventory}} | The actual occurrence of an entitled capability in the network |

This linkage ensures that refinement and occurrence formation have a tangible operational anchor in network management systems.


#Some specification examples
This section illustrates how equipment capability specifications connect to entitlement and inventory concepts.

Example covering an Optical Transponder:

1.  **Generic capability**: an abstract optical transponder supporting multiple modulation formats up to 800 G.
2.  **Equipment capability specification**: a vendor-specific model constrained to 400 G operation, defining port, thermal, and power envelopes.
3.  **Entitlement**: a software license enabling the 400 G feature set; represented via the entitlement model.
4.  **Inventory occurrence**: a deployed device instance that has the entitlement applied and exposes its active capabilities through inventory records.

This recursive narrowing from generic capability to entitled occurrence demonstrates how specification refinement is operationally realized.

Note that the use of the physical and functional considerations are recursively intertwined... a bit of physical, emergent function, function assembly, physical assembly thermals etc.

Note: Start with a generalized equipment and a generalized function and sketch the narrowing.

An equipment in a physical context considering how it fits and what fits in it...
-type
-size
-thermals
-power/thermal
-physical compatibility
-electrical compatibility
-diagram or compatibilities from ONF work
-physical assemblies and thermals

Function emergent from a physical equipment with applied data
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
Highlighting this general principle in terms of assemblies of equipment with applied data and the emergent behaviour that results.

#Generalization of the specification
Showing the reuse of specification fragments.

#Using the language of specification
This requires work in the generalized capabilities draft.

#Building the equipment specification structure
Take the language and general structure and build specific equipment.

#Conclusion
This document applies the generalized capability principles to the specific case of equipment with applied data.  By linking equipment capability descriptions to entitlements and inventory items, it creates a complete semantic chain from potential -> permitted -> realized.

This alignment ensures that planning, procurement, licensing, and operational systems can reason coherently about equipment functions and their lifecycle.  The approach enables automation, energy- and sustainability-aware network management, and AI-assisted reasoning grounded in formally defined capability structures.

#Security Considerations
TBD

#IANA Considerations

This document has no IANA actions.

--- back

#Acknowledgments

This document has been made with consensus and contributions coming from multiple drafts with different visions. We would like to thank all the participants in the IETF meeting discussions.

{:numbered="false"}
