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

...

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
{::include ./figures/bidirectional-link.txt}
~~~~
{: #fig-bidir-link title="Difference between one bidirectional link and two unidirectional links"}

## Multipoint Links

Multipoint links can be modelled as pseudonodes, as described in {{Section 4.4.5 of !RFC8345}}.

The type of multipoint link (e.g., point-to-multipoint or multipoint-to-multipoint, as defined in {{?I-D.ietf-nmop-simap-concept}}) and the role of the termination points in the link (e.g., source, destination, hub, spoke, as defined in {{?I-D.ietf-nmop-simap-concept}}) can be unambiguously understood from the links connecting the termination points of the pseudonode and from the 'is-allowed' attibute of the connectivity matrix, as defined in {{!RFC8795}}, of the pseudonode.

> Note: some examples for multipoint links are described in {{Section 2.5 of ?I-D.ietf-teas-te-topology-profiles}}. More examples can be provided in future versions of this document.

## Links and nodes down in topology

{{!RFC8795}} defines the 'oper-status' attribute for nodes, links and termination points, therefore allowing to report links and nodes down in the topology, and to unambiguously distinguishing between nodes and links which are up or down.

## Multi-Layer Topologies

As outlined in {{Section 2 of ?I-D.ietf-nmop-simap-concept}}:

> {{!RFC8345}} is flexible and can support both the same network topology instance with multiple layers (e.g., Layer 2 and Layer 3) or separate network topology instances with supporting relations between them (e.g., separate Layer 2 and Layer 3). Therefore, multiple topology layers can be grouped into the same network topology instance, if solution requires.

{{!RFC8795}} augments {{!RFC8345}} and therefore provide the same flexibility.

As described in {{Section 3 of ?I-D}}:
> - Layering relationships are expressed solely through the supporting
construct, without additional semantics such as underlay, primary,
backup, load-sharing, path, sequential, or parallel roles.

This issue was taken into account when designing the YANG data model in {{!RFC8795}} and the 'underlay' relationship has been properly defined to support navigation between overlay and underlay layers, as described in {{Section 2.3 of ?I-D.ietf-teas-te-topology-profiles}}.

Some guidelines on how to unambiguously use the supporting relationship, defined in {{!RFC8345}}, and the 'underlay' relationship defined in {{!RFC8795}}, have been clarified in {{Section 2.3.1 of ?I-D.ietf-teas-te-topology-profiles}}.

As outlined in REQ-BIDIR of {{?I-D.ietf-nmop-simap-concept}}, a bidirectional link can be supported by two unidirectional links in the lower layer. For example, a Ethernet (bidirectional) link can be supported by two physical layer links associated with two unidirectional fibers.

This relationship can be modelled using the link 'underlay' relationship: in this case each link is supported by a primary path composed by a single link.

## Multi-domain Links

Multi-domain links can be represented as open-ended links on each topology instance and unambiguously associated as multi-domain links using either the remote node ID / link ID attribute or the inter-domain-plug-id, as described in {{Section 4.2 of !RFC8795}}.

## Multi-domain Nodes

> Open issue: need further discussion about how to model the inter-area border nodes when belonging to different network topologies (one for each area)

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

{{?I-D.ietf-teas-te-topology-profiles}} indicates that the YANG deviation mechanism is not applicable for TE topology profiles since the provide to be supported may be different on different instances and it may depend on other attributes (e.g., the network type).

Existing implementations of {{!RFC8795}} describes the implemented profiled by manually pruning the YANG tree generated fom the YANG module defined in {{!RFC8795}}.

However, it is possible to use the YANG deviation statements to programmatically generate a profiled YANG tree.

An example of a YANG deviation module for SIMAP applications is provided below.

~~~~ yang
{::include yang/ietf-simap-deviation-example.yang}
~~~~
{: #fig-deviation-yang title="Example of SIMA deviation YANG module"
sourcecode-name="ietf-simap-deviation-example@2026-02-12.yang"}

The profiled YANG tree is provided below:

~~~~ ascii-art
{::include-fold yang/ietf-simap-deviation-example.tree}
~~~~

It is worth noting that even if an ad-hoc data model is defined, a coarse grain API would get more data than actually needed by an application.

For example, the following GET operation will get not only the data defined in {{!RFC8345}} but also all the data nodes defined in YANG data models which augment {{!RFC8345}} (e.g., technology-specific models):

~~~~ ascii-art
Add a GET operation for the data of a link

    GET  /restconf/data/ietf-network:networks/\
         /network=example:network/node=example:node-B/\
         /ietf-network-topology:termination-point=example:tp-2\
         / HTTP/1.1
    Host: example.com
    Accept: application/yang-data+json

    {
      "tp-id": "example:tp-2",
      "ietf-l3-unicast-topology:l3-termination-point-attributes": {
        "ip-address": [
          "192.0.2.4"
        ]
      }
    }

~~~~

An application that needs only a subset of the data nodes should either filter out the unnecessary information received from a coarse grain API or use a fine grained API to request only the data nodes that it needs.

> Action: investigate if it is possible to generate simple APIs from the YANG data model using the deviations

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
