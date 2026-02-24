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
# To be confirmed on the next call (2026-02-27)
#  -
#    name: Tarek Saad
#    org: Cisco Systems Inc.
#    email: tsaad.net@gmail.com

normative:

informative:

...

--- abstract

TODO Abstract


--- middle

# Introduction

TODO Introduction


# Conventions and Definitions

<!-- {::boilerplate bcp14-tagged} -->

TODO Conventions and Definitions

# Gap Analysis

## Bidirectional Links

Bidirectional links can be modelled as two unidirectional links, one for each direction,
terminated on the same two termination points, as described in {{Section 4.4.5 of !RFC8345}}.

It is worth noting that a bidirectional link can be unambiguously distinguished from two unidirectional links between the same two nodes since the two unidirectional links will be terminated on different termination points, as shown in {{fig-bidir-link}}.

~~~~ aasvg
{::include ./figures/bidirectional-link.txt}
~~~~
{: #fig-bidir-link title="Difference between one bidirectional link and two unidirectional links"}

> Open issue: discuss whether the bidirectional, which in turn might be supported as unidirectional links at the lower layer, is modelled using the supporting-link or the underlay path. Agreed in the 2026-02-20 TE call: use the underlay path and reference section 2.3.1 of the TE topology profile.

## Multipoint Links

Multipoint links can be modelled as pseudonodes, as described in {{Section 4.4.5 of !RFC8345}}.

The type of multipoint link (e.g., point-to-multipoint or multipoint-to-multipoint, as defined in {{?I-D.ietf-nmop-simap-concept}}) and the role of the termination points in the link (e.g., source, destination, hub, spoke, as defined in {{?I-D.ietf-nmop-simap-concept}}) can be unambiguously understood from the links connecting the termination points of the pseudonode and from the 'is-allowed' attibute of the connectivity matrix, as defined in {{!RFC8795}}, of the pseudonode.

> Note: some examples for multipoint links are described in {{Section 2.5 of ?I-D.ietf-teas-te-topology-profiles}}. More examples can be provided in future versions of this document.

## Multi-domain Links

Multi-domain links can be represented as open-ended links on each topology instance and unambiguously associated as multi-domain links using either the remote node ID / link ID attribute or the inter-domain-plug-id, as described in {{Section 4.2 of !RFC8795}}.

## Links and nodes down in topology

{{!RFC8795}} defines the 'oper-status' attribute for nodes, links and termination points, therefore allowing to report links and nodes down in the topology, and to unambiguously distinguishing between nodes and links which are up or down.

## Multi-Layer Topologies

> Open issue: need some brainstorming for the simple case where there is 1:1 mapping from the link in the client layer and the link in the server layer

## Termination points supported by physical devices

{{!RFC8345}} does not limit the type of components that can support a termination point.

The mapping between a termination point and an inventory component is under definition in {{!I-D.ietf-ivy-network-inventory-topology}}.

In the current version of {{!I-D.ietf-ivy-network-inventory-topology}}, a termination point can only be mappted to a port component. However, if needed, the IVY WG could update {{!I-D.ietf-ivy-network-inventory-topology}} to allow a termination point to be mapped to any type of component.

> Open issue: need further discussion since the example provided in {{?I-D.ietf-nmop-simap-concept}} is the IP loopback interface which cannot be mapped to a port component. However, the IP loopback interface is logical interface where IP packets are terminated (layer transition) so it is worthwhile discussing whether it is mapped as a termination point or as a tunnel termination point, as defined in {{!RFC8795}}.

# Design Considerations

Reusing existing YANG data models to support multiple applications is a key enabler to achieve multi-vendor interoperability.

A network controller needs to support multiple applications (some of which may not be defined at the time the network controller is defined) and different applications have different requirements on the data model they use internally to perform their tasks.

Exposing application-specific data models at the northbound of a network controller appears making the application simpler to implement when it is the only application to be implemented on top of a given controller but results in more complex implementations for network controllers and the applications and increase complexity to achieve multi-vendor interoperability and integration.

{{fig-as-dm}} shows a case where a controller-A is interfacing an Application-1 through the application-specific data model designed for Application-1 (i.e., AS1-DM) and a controller-B is interfacing another Application-2 through the application-specific data model designed for Application-2 (i.e., AS2-DM).

~~~~ aasvg
{::include ./figures/application-specific-data-model.txt}
~~~~
{: #fig-as-dm title="Concerns with defining application-specific data models"}

The two application-specific data models can provide the same information but with different formatting. For example, Application-1 may need to model bidirectional links as a single entity (link of type bidirectional) while Application-2 may need to model bidirectional links as two unidirectional links.

This solution works well as long as Application-1 and controller-A are deployed in different silos than Application-2 and controller-B. Otherwise, operational and implementation issues have to be faces when there is the need to run Application-1 over network controller-B or, Application-2 over network controller-A.

This would require the operator to negotiate with the vendor and controller vendors customized solutions before integrating a new application or a new controller in the network and increased complexity in the application and network controller implementations.

{{fig-common-dm}} describes an architecture based on a common and standardized data model to support multi-vendor integration.

~~~~ aasvg
{::include ./figures/common-standardized-data-model.txt}
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

> Agreed to describe the use of deviation statement as an example of how to do a coarse grain profiling of TE topology.

> Action: investigate if it is possible to generate simple APIs from the YANG data model using the deviations

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
