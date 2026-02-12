---
title: "Applicability of existing YANG data models to SIMAP"
abbrev: "SIMAP YANG"
category: info

docname: draft-xyz-nmop-simap-yang-applicability-latest
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

> Open issue: discuss whether the bidirectional, which in turn might be supported as unidirectional links at the lower layer, is modelled using the supporting-link or the underlay path

## Multipoint Links

Multipoint links can be modelled as pseudonodes, as described in {{Section 4.4.5 of !RFC8345}}.

The type of multipoint link (e.g., point-to-multipoint or multipoint-to-multipoint, as defined in {{?I-D.ietf-nmop-simap-concept}}) and the role of the termination points in the link (e.g., source, destination, hub, spoke, as defined in {{?I-D.ietf-nmop-simap-concept}}) can be unambiguously understood from the links connecting the termination points of the pseudonode and from the 'is-allowed' attibute of the connectivity matrix, as defined in {{!RFC8795}}, of the pseudonode.

> Note: some examples for multipoint links are described in {{?Section 2.5 of I-D.ietf-teas-te-topology-profiles}}. More examples can be provided in future versions of this document.

## Multi-domain Links

Multi-domain links can be represented as open-ended links on each topology instance and unambiguously associated as multi-domain links using either the remote node ID / link ID attribute or the inter-domain-plug-id, as described in {{!Section 4.2 of RFC8795}}.

## Links and nodes down in topology

{{!RFC8345}} does not explicitly indicate whether links and nodes down in the topology should be reported or not.

{{!RFC8795}} defines the 'oper-status' attribute for nodes, links and termination points, therefore allowing to report links and nodes down in the topology, and to unambiguously distinguishing between nodes and links which are up or down.

## Multi-Layer Topologies

> Open issue: need some brainstorming for the simple case where there is 1:1 mapping from the link in the client layer and the link in the server layer

## Termination points supported by physical devices

{{!RFC8345}} does not limit the type of components that can support a termination point.

The mapping between a termination point and an inventory component is under definition in {{!I-D.ietf-ivy-network-inventory-topology}}.

In the current version of {{!I-D.ietf-ivy-network-inventory-topology}}, a termination point can only be mappted to a port component. However, if needed, the IVY WG could update {{!I-D.ietf-ivy-network-inventory-topology}} to allow a termination point to be mapped to any type of component.

> Open issue: need further discussion since the example provided in {{?I-D.ietf-nmop-simap-concept}} is the IP loopback interface which cannot be mapped to a port component. However, the IP loopback interface is logical interface where IP packets are terminated (layer transition) so it is worthwhile discussing whether it is mapped as a termination point or as a tunnel termination point, as defined in {{!RFC8795}}.

# Design Considerations

> I think it would be worthwhile describing the possibility to transform the data model exposed by a network controller into an application-specific data model to be consumed by the application.

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Example of deviation statements

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
