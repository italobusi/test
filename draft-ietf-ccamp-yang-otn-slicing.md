---
title: "Framework and Data Model for OTN Network Slicing"
abbrev: "Framework and YANG of OTN Slices"
category: std

docname: draft-ietf-ccamp-yang-otn-slicing-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "Common Control and Measurement Plane"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "italobusi/test"
  latest: "https://italobusi.github.io/test/draft-ietf-ccamp-yang-otn-slicing.html"

author:
  -
    fullname: Aihua Guo
    org: Futurewei Technologies
    email: aihuaguo.ietf@gmail.com
  -
    fullname: Luis M. Contreras
    org: Telefonica
    email: luismiguel.contrerasmurillo@telefonica.com
  -
    fullname: Sergio Belotti
    org: Nokia
    email: Sergio.belotti@nokia.com
  -
    fullname: Reza Rokui
    org: Ciena
    email: rrokui@ciena.com
  -
    fullname: Yunbin Xu
    org: CAICT
    email: xuyunbin@caict.ca.cn
  -
    fullname: Yang Zhao
    org: China Mobile
    email: zhaoyangyjy@chinamobile.com
  -
    fullname: Xufeng Liu
    org: Alef Edge
    email: xufeng.liu.ietf@gmail.com

contributor:
  -
    fullname: Haomian Zheng
    org: Huawei Technologies
    street: H1, Xiliu Beipo Village, Songshan Lake
    city: Dongguan
    country: China
    email: zhenghaomian@huawei.com
  -
    fullname: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    fullname: Oscar Gonzalez de Dios
    ins: O. Gonzalez de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    fullname: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com
  -
    fullname: Dieter Beller
    org: Nokia
    email: Dieter.Beller@nokia.com
  -
    fullname: Henry Yu
    org: Huawei Technologies Canada
    email: henry.yu@huawei.com
  -
    fullname: Jiang Sun
    org: China Mobile
    email: sunjiang@chinamobile.com

normative:

informative:
  TS.28.530-3GPP:
    title: 3GPP TS 28.530 V15.1.0 Technical Specification Group Services and System
           Aspects; Management and orchestration; Concepts, use cases
           and requirements (Release 15)
    author:
      org: 3rd Generation Partnership Project (3GPP)
    date: December 2018
    seriesinfo: 3GPP TS 28.530
    target: http://ftp.3gpp.org//Specs/archive/28_series/28.530/28530-f10.zip
  ITU-T-G.8201-Amd.1:
    title: >
        Error performance parameters and objectives for multi-operator international
        paths within optical transport networks (Amendment 1)
    author:
      org: ITU Telecommunication Standardization Sector (ITU-T)
    date: December 2021
    seriesinfo: ITU-T G.8201 (2011) Amd. 1
    target: https://www.itu.int/rec/T-REC-G.8201

--- abstract

The requirement of slicing network resources with desired quality of
service is emerging at every network technology, including the
Optical Transport Networks (OTN). As a part of the transport network,
OTN can provide hard pipes with guaranteed data isolation and
deterministic low latency, which are highly demanded in the Service
Level Agreement (SLA).

This document describes a framework for OTN network slicing and defines
YANG data models with OTN technology-specific augments deployed at both
the north and south bound of the OTN network slice controller. Additional
YANG data model augmentations will be defined in a future version of
this draft.

--- middle

# Introduction

The requirement of slicing network resources with desired quality of
service is emerging at every network technology, including the
Optical Transport Networks (OTN). As a part of the transport network,
OTN can provide hard pipes with guaranteed data isolation and
deterministic low latency, which are highly demanded in the Service
Level Agreement (SLA).
This document describes a framework for OTN network slicing and defines
YANG data models with OTN technology-specific augments deployed at both
the north and south bound of the OTN network slice controller. Additional
YANG data model augmentations will be defined in a future version of
this draft.

## Terminology

{::boilerplate bcp14-tagged}

The terminology for describing YANG data models is found in {{!RFC7950}}.

## Prefixes in Data Node Names

In this document, names of data nodes and other data model objects
are prefixed using the standard prefix associated with the
corresponding YANG imported modules, as shown in {{tab-prefixes}}.

| Prefix   | YANG Module                  | Reference         |
|----------|------------------------------|-------------------|
| yang     | ietf-yang-types              | {{!RFC6991}}      |
| inet     | ietf-inet-types              | {{!RFC6991}}      |
| nt       | ietf-network-topology        | {{!RFC8345}}      |
| nw       | ietf-network-topology        | {{!RFC8345}}      |
| tet      | ietf-te-topology             | {{!RFC8795}}      |
| ietf-nss | ietf-network-slice-service   | \[RFCVVVV]        |
| ns-topo  | ietf-ns-topo                 | \[RFCWWWW]        |
| otnt     | ietf-otn-topology            | \[RFCYYYY]        |
| l1-types | ietf-layer1-types            | \[RFCZZZZ]        |
| otns     | ietf-otn-slice               | \[RFCXXXX]        |
| otns-mpi | ietf-otn-slice-mpi           | \[RFCXXXX]        |
{: #tab-prefixes title="Prefixes and Corresponding YANG Modules"}

RFC Editor Note:
Please replace VVVV with the RFC number assigned to {{!I-D.ietf-teas-ietf-network-slice-nbi-yang}}.
Please replace WWWW with the RFC number assigned to {{!I-D.ietf-teas-network-slice-topology-yang}}.
Please replace XXXX with the RFC number assigned to this document.
Please replace YYYY with the RFC number assigned to {{!I-D.ietf-ccamp-otn-topo-yang}}.
Please replace ZZZZ with the RFC number assigned to {{!I-D.ietf-ccamp-layer1-types}}.
Please remove this note.

## Definition of OTN Slice

An OTN slice is an an RFC 9543 Network Slice connecting a number
of OTN endpoints using a set of shared or dedicated OTN network resources to
satisfy specific service level objectives (SLOs) and Service Level Expectations (SLEs).

An OTN slice is a technology-specific realization of the RFC 9543 network slice service
{{?RFC9543}} in the OTN domain, with the
capability of configuring slice resources in the term of OTN technologies.
Therefore, all the terms and definitions concerning network slicing as
defined in {{?RFC9543}} apply to OTN slicing.

An OTN slice can span multiple OTN administrative domains, encompassing
access links, intra-domain paths, and inter-domain links.
An OTN slice may include multiple endpoints, each associated with a set of physical
or logical resources, e.g. optical port or time slots, at the termination point (TP) of
an access link or inter-domain link at an OTN provider edge (PE) equipment.

An end-to-end OTN slice may be composed of multiple OTN segment slices in
a hierarchical or sequential (or stitched) combination.

{{fig-otn-slice}} illustrates the scope of OTN slices in multi-domain
environment.

~~~~ aasvg
      <------------------End-to-end OTN Slice---------------->

      <- OTN Segment Slice 1 --->  <-- OTN Segment Slice 2 -->


       +-------------------------+  +-----------------------+
       | +-----+      +-------+  |  | +-------+      +-----+|
+----+ | | OTN |      | OTN   |  |  | | OTN   |      | OTN ||  +----+
| CE +-+-o PE  +-...--+ Borde o--+--+-o Borde +-...--+ PE  o+--+ CE |
+----+||/|     |      | Node  |\ || | | Node  |      |     || |+----+
      |||+-----+      +-------+| || | +-------+      +-----+| |
      |||    OTN Domain 1      | || |      OTN Domain 2     | |
      |++----------------------+-+| +-----------------------+ |
      | |                      |  |                           |
      | +-----+    +-----------+  |                           |
      |       |    |              |                           |
      V       V    V              V                           V
   Access    OTN Slice        Inter-domain                  Access
   Link      Endpoint         Link                          Link

~~~~
{: #fig-otn-slice title="OTN Slice"}

OTN slices may be pre-configured by the management plane and presented to
the customer via the northbound interface (NBI), or be dynamically
provisioned by a higher layer slice controller, e.g., an RFC 9543 network slice
controller (NSC) through the NBI. The OTN slice is
provided by a service provider to a customer to be used as though it was part
of the customer's own networks.

# Use Cases for OTN Network Slicing

## Leased Line Services with OTN

For end business customers (like OTT or enterprises), leased lines
have the advantage of providing high-speed connections with low
costs. On the other hand, the traffic control of leased lines is very
challenging due to rapid changes in service demands. Carriers are
recommended to provide network-level slicing capabilities to meet
this demand. Based on such capabilities, private network users have
full control over the sliced resources which have been allocated to them
and which could be used to support their leased lines, when needed.
Users may formulate policies based on the demand for services and
time to schedule the resources from the entire network's perspective
flexibly. For example, the bandwidth between any two points may be
established or released based on the time or monitored traffic
characteristics. The routing and bandwidth may be adjusted at a
specific time interval to maximize network resource utilization
efficiency.

## Co-construction and Sharing

Co-construction and sharing of a network are becoming a popular means
among service providers to reduce networking building CAPEX. For Co-
construction and sharing case, there are typically multiple co-
founders for the same network. For example, one founder may provide
optical fibres and another founder may provide OTN equipment, while
each occupies a certain percentage of the usage rights of the network
resources. In this scenario, the network O&M is performed by a
certain founder in each region, where the same founder usually
deploys an independent management and control system. The other
founders of the network use each other's management and control
system to provision services remotely. In this scenario, different
founders' network resources need to be automatically (associated)
divided, isolated, and visualized. All founders may share or have
independent O&M capabilities, and should be able to perform service-
level provisioning in their respective slices.

## Wholesale of optical resources

In the optical resource wholesale market, smaller, local carriers and
wireless carriers may rent resources from larger carriers, or
infrastructure carriers instead of building their networks. Likewise,
international carriers may rent resources from respective local
carriers and local carriers may lease their owned networks to each
other to achieve better network utilization efficiency.
From the perspective of a resource provider, it is crucial that a
network slice is timely configured to meet traffic matrix
requirements requested by its tenants. The support for multi-tenancy
within the resource provider's network demands that the network
slices are qualitatively isolated from each other to meet the
requirements for transparency, non-interference, and security.
Typically, a resource purchaser expects to use the leased network
resources flexibly, just like they are self-constructed. Therefore,
the purchaser is not only provided with a network slice, but also the
full set of functionalities for operating and maintaining the network
slice.  The purchaser also expects to, flexibly and independently,
schedule and maintain physical resources to support their own
end-to-end automation using both leased and self-constructed network
resources.

## Vertical dedicated network with OTN

Vertical industry slicing is an emerging category of network slicing
due to the high demand for private high-speed network interconnects
for industrial applications.
In this scenario, the biggest challenge is to implement
differentiated optical network slices based on the requirements from
different industries. For example, in the financial industry, to
support high-frequency transactions, the slice must ensure to provide
the minimum latency along with the mechanism for latency management.
For the healthcare industry, online diagnosis network and software
capabilities to ensure the delivery of HD video without frame loss.
For bulk data migration in data centers, the network needs to support
on-demand, large-bandwidth allocation. In each of the aforementioned
vertical industry scenarios, the bandwidth shall be adjusted as
required to ensure flexible and efficient network resource usage.

## End-to-end network slicing

In an end-to-end network slicing scenario such as 5G network slicing
{{TS.28.530-3GPP}}, an RFC 9543 network slice {{?RFC9543}}
provides the required connectivity between other different segments
of an end-to-end network slice, such as the Radio Access Network
(RAN) and the Core Network (CN) segments, with a specific
performance commitment. An RFC 9543 network slice could be composed of
network slices from multiple technological and administrative
domains. An RFC 9543 network slice can be realized by using or combining
multiple underlying OTN slices with OTN resources, e.g., ODU time
slots or ODU containers, to achieve end-to-end slicing across the transport
domain.

# Framework for OTN slicing

OTN slices may be abstracted differently depending on the requirement contained
in the configuration provided by the slice customer. Whereas the customer requests
an OTN slice to provide connectivity between specified endpoints, an OTN slice
can be abstracted as a set of endpoint-to-endpoint links, with each link formed
by an end-to-end tunnel across the underlying OTN networks. The resources
associated with each link of the slice are reserved and commissioned in the underlying
physical network upon the completion of configuring the OTN slice and all the
links are active.

An OTN slice can also be abstracted as an abstract topology when the customer requests
the slice to share resources between multiple endpoints and to use the resources on demand.
The abstract topology may consist of virtual nodes and virtual links{{!RFC9731}}, and their associated
resources are reserved but not commissioned across the underlying OTN networks. The
customer can later commission resources within the slice dynamically using the NBI provided
by the service provider. An OTN slice could use abstract topology to connect endpoints with
shared resources to optimize the resource utilization, and connections can be activated
within the slice as needed.

It is worth noting that those means to abstract an OTN slice are similar to the Virtual
Network (VN) abstraction defined for higher-level interfaces in {{!RFC8453}}, in which context
a connectivity-based slice corresponds to Type 1 VN and a resource-based slice corresponds to
Type 2 VN, respectively.

A particular resource in an OTN network, such as a port or link, may be
sliced with one of the two granularity levels:

-  Link-based slicing, in which a link and its associated link
termination points (LTPs) are dedicatedly allocated to a
particular OTN slice.

-  Tributary-slot based slicing, in which multiple OTN slices
share the same link by allocating different OTN tributary slots in
different granularities.

Furthermore, an OTN switch is typically fully non-blocking switching
at the lowest ODU container granularity, it is
desirable to specify just the total number of ODU containers in the
lowest granularity (e.g. ODU0), when configuring tributary-slot based
slicing on links and ports internal to an OTN network. In multi-domain
OTN network scenarios where separate OTN slices are created on
each of the OTN networks and are stitched at inter-domain OTN links, it
is necessary to specify matching tributary slots at the endpoints of the
inter-domain links. In some real network scenarios, OTN network resources
including tributary slots are managed explicitly by network operators for
network maintenance considerations. Therefore, an OTN slice controller
shall support configuring an OTN slice with both options.

An OTN slice controller (OTN-SC) is a logical function responsible for
the life-cycle management of OTN slices instantiated within the
corresponding OTN network domains. The OTN-SC provides technology-specific
interfaces at its northbound (OTN-SC NBI) to allow a higher-layer slice
controller, such as an RFC 9543 network slice controller (NSC) or an orchestrator,
to request OTN slices with OTN-specific
requirements. The OTN-SC interfaces at the southbound using the MDSC-to-PNC
interface (MPI) with a Physical Network Controller (PNC) or Multi-Domain Service Orchestrator (MDSC),
as defined in the ACTN control framework {{!RFC8453}}. The logical function
within the OTN-SC is responsible for translating the OTN slice requests
into concrete slice realization which can be understood and
provisioned at the southbound by the PNC or MDSC.

The presence of OTN-SC provides multiple options for a high-level slice controller
or an orchestrator to configure and realize slicing in OTN networks, depending on
whether a customer's slice request is technology agnostic or technology specific:

- Option 1\[opt.1]: An IETF NSC receives a technology-agnostic slice request from the IETF NSC NBI and
realizes full or part of the slice in OTN networks directly through MPI provided by
the PNC or MDSC. The IETF NSC is responsible for mapping a technology-agnostic slicing request
into an OTN technology-specific realization. In this option, the OTN-SC is not used.

- Option 2\[opt.2]: An IETF NSC receives a technology-agnostic slice request from the IETF NSC NBI and delegates the
request to the OTN-SC through the OTN-SC NBI, which is OTN technology specific. The OTN-SC in turn realizes the slice in single or multi domain OTN networks by working with the underlying PNC or MDSC. In this option, the OTN-SC is considered as a realization of IETF NSC, i.e.,
an NS realizer as per {{!I-D.ietf-teas-ns-controller-models}},
when the underlying network is OTN. The OTN-SC is also a subordinate slice controller of the RFC 9543 NSC, which
is consistent with the hierarchical control of slices specified by {{?RFC9543}}.

- Option 3\[opt.3]: An OTN-aware orchestrator may request an OTN technology-specific slice with OTN-specific SLOs through the
OTN-SC NBI to the OTN-SC. The OTN-SC in turn realizes the slice in single or multi domain OTN networks by working with the underlying PNC or MDSC

An OTN slice may be realized by using standard MPI interfaces, control plane, network management system (NMS) or any other proprietary interfaces as needed. Examples of such interfaces include the abstract TE topology {{!RFC8795}}, TE tunnel {{!I-D.ietf-teas-yang-te}},L1VPN{{!RFC4847}}, or Netconf/YANG based interfaces such as OpenConfig. Some of these interfaces, such as the TE tunnel model, are suitable for creating connectivity-based OTN slices which represent a slice as a set of TE tunnels, while other interfaces such as the TE topology model are more suitable for creating resource-based OTN slices which represent a slice as a topology.

The OTN-SC NBI is a technology-specific interface that augments the IETF NSC NBI, which is technology-
agnostic.

{{fig-slice-interfaces}} illustrates the OTN slicing control hierarchy, the positioning of the OTN slicing interfaces as well as the options for OTN slice configuration.

~~~~ aasvg
                      +--------------------+
                      | Provider's User    |
                      +--------|-----------+
                               | CMI
       +-----------------------+--------------------------------+
       |          Orchestrator / E2E Slice Controller           |
       +------------+-----------------------------+-------------+
                    |                             | NSC-NBI
                    |       +---------------------+-------------+
                    |       | RFC 9543 Network Slice Controller |
                    |       +-----+---------------+-------------+
                    | opt.3       | opt.2         | opt.1
                    | OTN-SC NBI  |OTN-SC NBI     |
       +------------+-------------+--------+      |
       |               OTN-SC              |      |
       +--------------------------+--------+      |
                                  | MPI           | MPI
       +--------------------------+---------------+------------+
       |                         PNC                           |
       +--------------------------+----------------------------+
                                  | SBI
                      +-----------+----------+
                      |OTN Physical Network  |
                      +----------------------+

~~~~
{: #fig-slice-interfaces title="Positioning of OTN Slicing Interfaces"}

OTN-SC functionalities may be recursive such that a higher-level
OTN-SC may designate the creation of OTN slices to a lower-level
OTN-SC in a recursive manner. This scenario may apply to the
creation of OTN slices in multi-domain OTN networks, where
multiple domain-wide OTN slices provisioned by lower-layer
OTN-SCs are stitched to support a multi-domain OTN slice
provisioned by the higher-level OTN-SC.  Alternatively, the OTN-SC
may interface with an MDSC, which in turn interfaces with multiple
PNCs through the MPI to realize OTN slices in multi-domain OTN networks
without OTN-SC recursion.
{{fig-otn-sc-recursion}} illustrates both options for OTN slicing
in multi-domain.

~~~~ aasvg
    +-------------------+                    +-------------------+
    |      OTN-SC       |                    |      OTN-SC       |
    +--------|----------+                    +---|----------|----+
             |MPI                                |OTN-SC NBI|
    +--------|----------+                    +---|----+ +---|----+
    |      MDSC         |                    | OTN-SC | | OTN-SC |
    +---|----------|----+                    +---|----+ +---|----+
        |MPI       |MPI                          |MPI       |MPI
    +---|----+ +---|----+                    +---|----+ +---|----+
    |   PNC  | |   PNC  |                    |   PNC  | |   PNC  |
    +--------+ +--------+                    +--------+ +--------+
    Multi-domain Option 1                    Multi-domain Option 2
~~~~
{: #fig-otn-sc-recursion title="OTN-SC for multi-domain"}

OTN-SC functionalities are logically independent and may be deployed in
different combinations to cater to the realization needs. In reference to the
ACTN control framework {{!RFC8453}}, an OTN-SC may be deployed

- as an independent network function;

- together with a Physical Network Controller (PNC) for single-domain
or with a Multi-Domain Service Orchestrator (MDSC)for multi-domain;

- together with a higher-level network slice controller to support
end-to-end network slicing;

# Realizing OTN Slices

{{?RFC9543}} introduces a mechanism for an RFC 9543 network slice controller to realize network slices by constructing Network Resource Partitions (NRP). A NRP is a collection of resources identified in the underlay network to facilitate the mapping of network slices onto available network resources. An NRP is a scope view of a topology and may be considered as a topology in its own right. Thus, in traffic-engineered (TE) networks including OTN, an NRP may be simply represented as an abstract TE topology defined by {{!RFC8795}}. For OTN networks, An NRP may be represented as an abstract OTN topology defined by {{!I-D.ietf-ccamp-otn-topo-yang}}.

The NRP can be used to address scalability challenges that arise when network slices are mapped directly onto the underlay topology, where a large number of control-plane and data-plane states may otherwise need to be instantiated and maintained for each slice. An NRP is internal to the network slice controller, and its use is optional and particularly in OTN transport networks, where resources are already physically partitioned into time slots with corarse granularity than the resources considered in L2-L3 networks. Nevertheless, NRPs can provide significant benefits for slice realization in large-scale environments, including also OTN networks.

For connectivity-based OTN slices, a connection within an OTN slice is typically realized by an OTN tunnel in the underlay topology and resources are reserved by the tunnel, thus use of NRP is optional in this case.

For resource-based OTN slices, the OTN-SC maps an OTN slice directly onto the underlay TE topology exposed by the subtended controller (MDSC or PNC) without creating separate NRP topology instances. In this case, the OTN-SC configures NRP identifiers on the relevant underlay link resources. An NRP identifier represents a resource partition with ODU time slots for tracking slice resource associations. The OTN-SC may then push the topology with configured NRP identifiers to the subtended MDSC or PNC using the MPI model defined in this draft, and subsequently instantiate OTN TE tunnels utilizing the related underlay link resources with the appropriate NRP identifier.

Multiple OTN slices may be mapped to the same NRP, while any individual connectivity construct of a slice is mapped to only one NRP, as specified in {{?RFC9543}}. The resources of an NRP topology are reserved and shared among all OTN slices mapped to the same NRP.

{{fig-otn-sc-nrp}} illustrates the relationship between OTN slices and NRPs. In this example, Slice 1 and Slice 2 are associated with NRP-1, while Slice 3 is associated with NRP-2. The relevant link resources allocated to each NRP are marked with their corresponding NRP identifiers.

~~~~ aasvg
        /---------------/                   /---------------/
       /  --     --    /                   /  --     --    /
      /  |N1|---|N3|  /---/               /  |N1|   |N3|  /
     /    --\    --  /   /               /    --     --  /
    /        \--    /   /               /       \ --/   /
   /         |N2|  /   /               /         |N2|  /
  / Slice 1   --  /   /               / Slice 3   --  /
 /------------|--/   /               /-----------|---/
    / Slice 2 |     /                            |
   /--------- |-|--/                             |
+-------------v-v--------------------------------v--------+
|          (NRP-1)                            (NRP-2)     |
|                                                         |
|             /-----------------------------/             |
|            / /--\    NRP-1    /--\       /              |
|           / |NE1 |-----------|NE2 |     /               |
|          /   ---/\           /\--/     /                |
|         /   /     \NRP-1    /NRP-2    /                 |
|        / /-/\      \ /--\  /         /                  |
|       / |NE3 |------|NE4 |/         /                   |
|      /   \--/  NRP-2 \--/          /                    |
|     /                             /                     |
|    / Underlay OTN TE Topology    /                      |
|   /  with NRP identifier marking/                       |
|  /-----------------------------/                        |
|                      OTN-SC                             |
+---------------------------------------------------------+
                 |                         ^
                 |MPI                      |MPI
+----------------V----------------------------------------+
|                                                         |
|                       OTN MDSC/PNC                      |
+---------------------------------------------------------+
~~~~
{: #fig-otn-sc-nrp title="Mapping OTN Slices to NRP"}

# YANG Data Model for OTN Slicing Configuration

## OTN Slicing YANG Model for MPI

### MPI YANG Model Overview

For the realization of connectivity-based OTN slices, the OTN-SC configures an OTN tunnel using the OTN tunnel model and specifies the associated NRP identifier.

For the realization of resource-based OTN slices, the OTN-SC configures the NRP by marking the relevant link resources on the TE topology received from the MDSC or PNC with an NRP identifier, together with the appropriate OTN-specific resource attributes, such as the number of ODU time slots or the type and quantity of ODU containers.

Based on the resources marked by the OTN-SC, the MDSC or PNC updates the underlay TE topology by creating new TE links corresponding to the reserved OTN resources and keeping those resources booked for the slice.

### MPI YANG Model Tree

~~~~
{::include-fold yang/trees/ietf-otn-slice-mpi.tree}
~~~~
{: #fig-otn-slice-mpi-tree title="OTN slicing MPI tree diagram"}

### MPI YANG Code

~~~~ yang
{::include yang/ietf-otn-slice-mpi.yang}
~~~~
{: #fig-otn-slice-mpi-yang title="OTN slicing MPI YANG model"
sourcecode-markers="true" sourcecode-name="ietf-otn-slice-mpi@2025-07-03.yang"}

## OTN Slicing YANG Model for OTN-SC NBI

### NBI YANG Model Overview

The YANG model for OTN-SC NBI is OTN-technology specific, but shares many
common constructs and attributes with the common network slicing YANG model
defined in {{!I-D.ietf-teas-ietf-network-slice-nbi-yang}}. Furthermore, the
OTN-SC NBI YANG is expected to support both connectivity-based
and resource-based slice configuration, which is likely a common requirement for
supporting slicing at other transport network layers, e.g. WDM or MPLS(-TP).

The OTN slicing model augments the common network slicing YANG model by extending
OTN technology-specific SLO and SLE attributes which can be requested by OTN-aware
customers and allows the customer to specify desired OTN signal quality.
These attributes include:

- The performance objective for Optical Data Unit (ODU) containers as defined in
{{ITU-T-G.8201-Amd.1}}.

- Bandwidth specification in the type and number of ODU containers.

### NBI YANG Model Tree for OTN slice

~~~~
{::include-fold yang/trees/ietf-otn-slice.tree}
~~~~
{: #fig-ietf-otn-slice title="Tree diagram for OTN slice"}

### NBI YANG Code for OTN Slice

~~~~ yang
{::include yang/ietf-otn-slice.yang}
~~~~
{: #fig-ietf-otn-slice-yang title="YANG model for transport network slice"
sourcecode-markers="true" sourcecode-name="ietf-otn-slice@2026-03-18.yang"}

# Manageability Considerations

To ensure the security and controllability of physical resource
isolation, slice-based independent operation and management are
required to achieve management isolation.
Each optical slice typically requires dedicated accounts,
permissions, and resources for independent access and O&M. This
mechanism is to guarantee the information isolation among slice
tenants and to avoid resource conflicts. The access to slice
management functions will only be permitted after successful security
checks.

# Operational Considerations

TBC per {{?I-D.opsarea-rfc5706bis}}: Manageability Considerations?

# Security Considerations

The YANG module specified in this document defines a schema for data
that is designed to be accessed via network management protocols such
as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer
is the secure transport layer, and the mandatory-to-implement secure
transport is Secure Shell (SSH) {{!RFC6242}}.  The lowest RESTCONF layer
is HTTPS, and the mandatory-to-implement secure transport is TLS
{{!RFC8446}}.

The NETCONF access control model {{!RFC8341}} provides the means to
restrict access for particular NETCONF or RESTCONF users to a
preconfigured subset of all available NETCONF or RESTCONF protocol
operations and content.

There are a number of data nodes defined in this YANG module that are
writable/creatable/deletable (i.e., config true, which is the
default).  These data nodes may be considered sensitive or vulnerable
in some network environments.  Write operations (e.g., edit-config)
to these data nodes without proper protection can have a negative
effect on network operations.  Considerations in Section 8 of
{{!RFC8795}} are also applicable to their subtrees in the module defined
in this document.

Some of the readable data nodes in this YANG module may be considered
sensitive or vulnerable in some network environments.  It is thus
important to control read access (e.g., via get, get-config, or
notification) to these data nodes.  Considerations in Section 8 of
{{!RFC8795}} are also applicable to their subtrees in the module defined
in this document.

# IANA Considerations

It is proposed to IANA to assign new URIs from the "IETF XML
Registry" {{!RFC3688}} as follows:

~~~~
   URI: urn:ietf:params:xml:ns:yang:ietf-otn-slice
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.

   URI: urn:ietf:params:xml:ns:yang:ietf-otn-slice-mpi
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.
~~~~

This document registers two YANG modules in the YANG Module Names
registry {{!RFC6020}}.

~~~~
   name: ietf-otn-slice
   namespace: urn:ietf:params:xml:ns:yang:ietf-otn-slice
   prefix: otns
   reference: RFC XXXX

   name: ietf-otn-slice-mpi
   namespace: urn:ietf:params:xml:ns:yang:ietf-otn-slice-mpi
   prefix: otns-mpi
   reference: RFC XXXX
~~~~

--- back

# Acknowledgments
{:numbered="false"}

This document was prepared using kramdown.

Previous versions of this document were prepared using 2-Word-v2.0.template.dot.

The authors would like to thank Adrian Farrel, Danielle Ceccarelli,
Igor Bryskin, Bo Wu, Gyan Mishra, Joel M. Halpen, Dhruv Dhoddy and Loa Andersson
for providing valuable insights.
