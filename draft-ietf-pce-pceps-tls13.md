---
title: Updates for PCEPS
abbrev: Updates for PCEPS
category: std
updates: 8253

docname: draft-ietf-pce-pceps-tls13-latest
submissiontype: IETF
consensus: true
v: 3
area: "Routing"
workgroup: "PCE"
keyword:
 - PCEP
 - PCEPS
 - TLS 1.3
 - TLS 1.2
 - Early Data
 - 0-RTT

author:
 -
    ins: D. Dhody
    name: Dhruv Dhody
    organization: Huawei Technologies
    email: dhruv.ietf@gmail.com
 -
    ins: S. Turner
    name: Sean Turner
    organization: sn3rd
    email: sean@sn3rd.com
 -
    name: Russ Housley
    org: Vigil Security, LLC
    abbrev: Vigil Security
    street: 516 Dranesville Road
    city: Herndon, VA
    code: 20170
    country: US
    email: housley@vigilsec.com

--- abstract

RFC 8253 defines how to protect PCEP messages with TLS 1.2. This
document updates RFC 8253 to address support requirements for TLS 1.2
and TLS 1.3 and the use of TLS 1.3's early data.


--- middle

# Introduction

{{!RFC8253}} defines how to protect PCEP messages {{!RFC5440}} with
TLS 1.2 {{!RFC5246}}. This document updates {{RFC8253}} to address
support requirements for TLS 1.2 {{RFC5246}} and TLS 1.3 {{!I-D.ietf-tls-rfc8446bis}}
and the use of TLS 1.3's early data, which is also known as 0-RTT data.
All other provisions set forth in {{RFC8253}} are unchanged, including
connection initiation, message framing, connection closure, certificate
validation, peer identity, and failure handling.

<aside markdown="block">
  Editor's Note: The reference to {{I-D.ietf-tls-rfc8446bis}} could
  be changed to RFC 8446 incase the progress of the bis draft is
  slower than the progression of this document.
</aside>


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Early Data

Early data (aka 0-RTT data) is a mechanism defined in TLS 1.3
{{I-D.ietf-tls-rfc8446bis}} that allows a client to send data ("early data")
as part of the first flight of messages to a server. Note that
TLS 1.3 can be used without early data as per
{{Section F.5 of I-D.ietf-tls-rfc8446bis}}.
In fact, early data is permitted by TLS 1.3 only when the client and server
share a Pre-Shared Key (PSK), either obtained
externally or via a previous handshake. The client uses the PSK to
authenticate the server and to encrypt the early data.

As noted in {{Section 2.3 of I-D.ietf-tls-rfc8446bis}}, the security
properties for early data are weaker than those for subsequent TLS-protected
data. In particular, early data is not forward secret, and there is no
protection against the replay of early data between connections.
{{Appendix E.5 of I-D.ietf-tls-rfc8446bis}} requires applications not
use early data without a profile that defines its use. This document
specifies that PCEPS implementations that support TLS 1.3 MUST NOT use early data.


# Cipher Suites

Implementations MUST support TLS 1.2 {{RFC5246}} and are REQUIRED to
support the TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 cipher suite {{!RFC9325}}.

Implementations MAY implement additional TLS 1.2 cipher suites that provide
mutual authentication and confidentiality as required by PCEP.

Implementations SHOULD support TLS 1.3 {{I-D.ietf-tls-rfc8446bis}} and,
if implemented, MUST prefer to negotiate TLS 1.3 over earlier versions
of TLS.

Implementations that support TLS 1.3 {{I-D.ietf-tls-rfc8446bis}}
are REQUIRED to support the mandatory-to-implement cipher
suites listed in {{Section 9.1 of I-D.ietf-tls-rfc8446bis}}.

Implementations that support TLS 1.3 MAY implement additional TLS 1.3
cipher suites that provide mutual authentication and confidentiality
as required by PCEP.

PCEPS Implementations SHOULD follow the recommendations given in
{{RFC9325}}.


# Security Considerations

The Security Considerations of PCEP {{RFC5440}}, {{?RFC8231}},
{{?RFC8281}}, and {{?RFC8283}}; TLS 1.2 {{RFC5246}}; TLS 1.3 {{I-D.ietf-tls-rfc8446bis}},
and; {{RFC9325}} apply here as well.

The Path Computation Element (PCE) defined in {{?RFC4655}} is an entity
that is capable of computing a network path or route based on a
network graph, and applying computational constraints.  A Path
Computation Client (PCC) may make requests to a PCE for paths to be
computed. PCEP is the communication protocol between a PCC and PCE and is
defined in {{RFC5440}}. Stateful PCE {{RFC8231}} specifies a set of extensions to PCEP to
enable control of TE-LSPs by a PCE that retains the state of the LSPs
provisioned in the network (a stateful PCE).  {{RFC8281}} describes the
setup, maintenance, and teardown of LSPs initiated by a stateful PCE
without the need for local configuration on the PCC, thus allowing
for a dynamic network that is centrally controlled.  {{RFC8283}}
introduces the architecture for PCE as a central controller

TLS mutual authentication is used to ensure that only authorized users
and systems are able to send and receive PCEP messages. To this end,
neither the PCC nor the PCE should establish a PCEPS with TLS connection
with an unknown, unexpected, or incorrectly identified peer; see
{{Section 3.5 of RFC5440}}. If deployments make use of a trusted list of
Certification Authority (CA) certificates {{!RFC5280}}, then the listed
CAs should only issue certificates to parties that are authorized to
access the PCE. Doing otherwise will allow certificates that were issued
for other purposes to be inappropriately accepted by a PCE.


# IANA Considerations

There are no IANA considerations.

# Implementation Status

[Note to the RFC Editor - remove this section before publication, as well as remove the reference to RFC 7942.]

This section records the status of known implementations of the
protocol defined by this specification at the time of posting of this
Internet-Draft, and is based on a proposal described in {{RFC7942}}.
The description of implementations in this section is intended to
assist the IETF in its decision processes in progressing drafts to
RFCs.  Please note that the listing of any individual implementation
here does not imply endorsement by the IETF.  Furthermore, no effort
has been spent to verify the information presented here that was
supplied by IETF contributors.  This is not intended as, and must not
be construed to be, a catalogue of available implementations or their
features.  Readers are advised to note that other implementations may
exist.

According to {{RFC7942}}, "this will allow reviewers and working groups
to assign due consideration to documents that have the benefit of
running code, which may serve as evidence of valuable experimentation
and feedback that have made the implemented protocols more mature.
It is up to the individual working groups to use this information as
they see fit".

At the time of posting the -01 version of this document, there are no
known implementations of this mechanism.


--- back

# Acknowledgments
{:numbered="false"}

We would like to thank Adrian Farrel for their review.
