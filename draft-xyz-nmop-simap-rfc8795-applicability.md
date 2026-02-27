---
title: "Applicability of RFC8795 YANG data model to SIMAP"
abbrev: "RFC8795 for SIMAP"
category: info

docname: draft-xyz-nmop-simap-rfc8795-applicability-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Network Management Operations"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Network Management Operations"
  type: "Working Group"
  mail: "nmop@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/nmop/"
  github: "italobusi/simap-yang"
  latest: "https://italobusi.github.io/simap-yang/draft-xyz-nmop-simap-yang-applicability.html"

author:
  -
    name: Italo Busi
    org: Huawei
    email: italo.busi@huawei.com
  -
    name: Aihua Guo
    org: Futurewei Technologies
    email: aihuaguo.ietf@gmail.com
  -
    name: Vishnu Pavan Beeram
    org: Juniper Networks
    email: vbeeram@juniper.net
  -
    name: Sergio Belotti
    org: Nokia
    email: sergio.belotti@nokia.com
# To be confirmed on the next call (2026-02-27)
#  -
#    name: Tarek Saad
#    org: Cisco Systems Inc.
#    email: tsaad.net@gmail.com

normative:

informative:

--- abstract

This document analyses the applicability of the RFC 8795 YANG data model to
Service & Infrastructure Maps (SIMAP) and in particular analysis which requirements
can be supported by the existing YANG data model defined in RFC 8795.

--- middle

# Introduction

The concept of Service & Infrastructure Maps (SIMAP) is being defined in {{?I-D.ietf-nmop-simap-concept}}
together with a set of SIMP requirements and use cases.

SIMAP is defined in {{?I-D.ietf-nmop-simap-concept}}, as:

> SIMAP is a data model that provides a topological view of the operator's networks and services, including how it is connected to other models (e.g., inventory) and external data sources (e.g., observability data, and operational knowledge). This model represents a multi-layered topology and offers mechanisms to navigate amongst layers and correlate between them. This includes layers from physical topology to service topology. This model is applicable to multiple domains (access, core, data center, etc.) and technologies (Optical, IP, etc.).

It is worth noting that the YANG data model in {{!RFC8795}} defines a topology model which:

- represents a multi-layered topology;
- offers mechanisms to navigate amongst layers and correlate between them;
- is applicable to multiple domains (access, core, data center, etc.) and technologies (Optical, IP, etc.).

{{?I-D.ietf-teas-te-topology-profiles}} clarifies that

It is therefore worthwhile analyzing the applicability of the YANG data model in {{!RFC8795}} to meet the SIMAP requirements and identify any gaps
that need to be addressed before starting the work on new YANG data models to meet the SIMAP requirements, as outlined in {{?I-D.ietf-nmop-simap-concept}}.

Understanding the degree of standardization and identifying potential gaps are crucial for supporting
the SIMAP applications while ensuring multi-vendor interoperability and compatibility with other applications
which can run on top of the same network controller.

# Conventions and Definitions

<!-- {::boilerplate bcp14-tagged} -->

TODO Conventions and Definitions

# Gap Analysis

## Bidirectional Links

Bidirectional links can be modelled as two unidirectional links, one for each direction,
terminated on the same two termination points, as described in {{Section 4.4.5 of !RFC8345}}.

It is worth noting that a bidirectional link can be unambiguously distinguished from two unidirectional links between the same two nodes since the two unidirectional links will be terminated on different termination points, as shown in {{fig-bidir-link}}.

~~~~ aasvg
{::include figures/bidirectional-unidirectional-links.txt}
~~~~
{: #fig-bidir-link title="Difference between one bidirectional link and two unidirectional links"}

## Multipoint Links

Multipoint links can be modelled as pseudonodes, as described in {{Section 4.4.5 of !RFC8345}}.

The type of multipoint link (e.g., point-to-multipoint or multipoint-to-multipoint, as defined in {{?I-D.ietf-nmop-simap-concept}}) and the role of the termination points in the link (e.g., source, destination, hub, spoke, as defined in {{?I-D.ietf-nmop-simap-concept}}) can be unambiguously understood from:
- the links entering to or departing from the termination points of the pseudonode, and
- the 'is-allowed' attribute of the connectivity matrix of the pseudonode, as defined in {{!RFC8795}}.

> Note: some examples for multipoint links are described in {{Section 2.5 of ?I-D.ietf-teas-te-topology-profiles}}. More examples can be provided in future versions of this document.

## Links and nodes down in topology

{{!RFC8795}} defines the 'oper-status' attribute for nodes, links and termination points, therefore allowing to report links and nodes down in the topology, and to unambiguously distinguishing between nodes and links which are up or down.

## Multi-Layer Topologies

As outlined in {{Section 2 of ?I-D.ietf-nmop-simap-concept}}:

> {{!RFC8345}} is flexible and can support both the same network topology instance with multiple layers (e.g., Layer 2 and Layer 3) or separate network topology instances with supporting relations between them (e.g., separate Layer 2 and Layer 3). Therefore, multiple topology layers can be grouped into the same network topology instance, if solution requires.

{{!RFC8795}} augments {{!RFC8345}} and therefore provide the same flexibility.

As described in {{Section 3 of ?I-D.havel-nmop-simap-yang}}:

> - Layering relationships are expressed solely through the supporting
construct, without additional semantics such as underlay, primary,
backup, load-sharing, path, sequential, or parallel roles.

This issue was taken into account when designing the YANG data model in {{!RFC8795}} and the 'underlay' relationship has been properly defined to support navigation between overlay and underlay layers, as described in {{Section 2.3 of ?I-D.ietf-teas-te-topology-profiles}}.

Some guidelines on how to unambiguously use the supporting relationship, defined in {{!RFC8345}}, and the 'underlay' relationship defined in {{!RFC8795}}, have been clarified in {{Section 2.3.1 of ?I-D.ietf-teas-te-topology-profiles}}.

As outlined in REQ-BIDIR of {{?I-D.ietf-nmop-simap-concept}}, a bidirectional link can be supported by two unidirectional links in the lower layer. For example, a Ethernet (bidirectional) link can be supported by two physical layer links associated with two unidirectional fibers.

This relationship can be modelled using the link 'underlay' relationship, as shown in {{fig-unidir-supporting-bidir-link}}:

~~~~ aasvg
{::include figures/unidir-supporting-bidir-link.txt}
~~~~
{: #fig-unidir-supporting-bidir-link title="Example of two unidirectional links supporting one bidirectional link"}

Note: in this case each link is supported by a primary path composed by a single link.

It is worth noting that modelling the bidirectional link is modelled as two unidirectional link instances allows also to unambiguously understand which unidirectional underlay link/path supports which direction (forward or reverse) of the overlay bidirectional link.
ls
## Multi-domain Links

Multi-domain links can be represented as open-ended links on each topology instance and unambiguously associated as multi-domain links using either the remote node ID / link ID attribute or the inter-domain-plug-id, as described in {{Section 4.2 of !RFC8795}}.

# Design Considerations

Reusing existing YANG data models to support multiple applications is a key enabler to achieve multi-vendor interoperability.

A network controller needs to support multiple applications (some of which may not be defined at the time the network controller is defined) and different applications have different requirements on the data model they use internally to perform their tasks.

Exposing application-specific data models at the northbound of a network controller appears making the application simpler to implement when it is the only application to be implemented on top of a given controller but results in more complex implementations for network controllers and the applications and increase complexity to achieve multi-vendor interoperability and integration.

{{fig-as-dm}} shows a case where a controller-A is interfacing an Application-1 through the application-specific data model designed for Application-1 (i.e., AS1-DM) and a controller-B is interfacing another Application-2 through the application-specific data model designed for Application-2 (i.e., AS2-DM).

~~~~ aasvg
{::include figures/application-specific-data-model.txt}
~~~~
{: #fig-as-dm title="Concerns with defining application-specific data models"}

The two application-specific data models can provide the same information but with different formatting. For example, Application-1 may need to model bidirectional links as a single entity (link of type bidirectional) while Application-2 may need to model bidirectional links as two unidirectional links.

This solution works well as long as Application-1 and controller-A are deployed in different silos than Application-2 and controller-B. Otherwise, operational and implementation issues have to be faces when there is the need to run Application-1 over network controller-B or, Application-2 over network controller-A.

This would require the operator to negotiate with the vendor and controller vendors customized solutions before integrating a new application or a new controller in the network and increased complexity in the application and network controller implementations.

{{fig-common-dm}} describes an architecture based on a common and standardized data model to support multi-vendor integration.

~~~~ aasvg
{::include figures/common-standardized-data-model.txt}
~~~~
{: #fig-common-dm title="Use of a common and standardized data model to support multi-vendor integration"}

In this case, the data model exposed by network controllers (e.g., controller-A and controller-B) is the same and each application needs to translate/map the standardized data model exposed by the network controller to its own application-specific data model.

Following this approach an existing application (e.g., Application-1) can be ported from one controller to another controller (e.g., from controller-A to controller-B) ideally with no change and additional testing.

Reusing RFC8795 YANG data model for SIMAP applications will allow TE and SIMAP application to co-exist on top of the same network controller, allowing seamless migration from network scenarios supporting only on of these application to network scenarios where both applications are supported at the same time or even scenarios where either or both applications are supported together with new applications not yet defined.

# Security Considerations

TODO Security

# IANA Considerations

This document has no IANA actions.

--- back

# Example of deviation statements

{{?I-D.ietf-teas-te-topology-profiles}} notes that existing implementations of {{!RFC8795}} have described the implemented profiled by manually pruning/profiling the YANG tree generated fom the YANG module defined in {{!RFC8795}} and those pruned/profiled YANG trees provided sufficient input for the implementers to generate proper APIs.

However, {{?I-D.ietf-teas-te-topology-profiles}} is also exploring the possibility to use the YANG deviation statements to programmatically generate pruned/profiled YANG trees.

An example of a YANG deviation module for SIMAP applications is provided below.

~~~~ yang
{::include yang/ietf-simap-deviation-example.yang}
~~~~
{: #fig-deviation-yang title="Example of SIMA deviation YANG module"
sourcecode-name="ietf-simap-deviation-example@2026-02-12.yang"}

The pruned/profiled YANG tree, generated using 'pyang' tool, is provided below:

~~~~ ascii-art
{::include-fold yang/ietf-simap-deviation-example.tree}
~~~~

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
