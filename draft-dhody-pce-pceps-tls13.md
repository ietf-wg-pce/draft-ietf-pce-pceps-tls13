---
title: PCEPS with TLS 1.3
abbrev: PCEPS-with-TLS1.3
category: std

docname: draft-dhody-pce-pceps-tls13-latest
submissiontype: IETF
consensus: true
v: 3
area: "Routing"
workgroup: "PCE"
keyword:
 - PCEP
 - PCEPS
 - TLS 1.3

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

RFC 8253 defines how to protect PCEP messages with TLS 1.2.
This document describes how to protect PCEP messages with TLS 1.3.


--- middle

# Introduction

{{!RFC8253}} defines how to protect PCEP messages {{!RFC5440}} with
TLS 1.2 {{?RFC5246}}. This document describes defines how to protect
PCEP messages with TLS 1.3 {{!I-D.ietf-tls-rfc8446bis}}.

[Editor's Note: The reference to {{I-D.ietf-tls-rfc8446bis}} could be 
changed to RFC 8446 incase the progress of the bis draft is slower than the 
progression of this document.]

This document addresses cipher suites and the use of early data, which is also
known as 0-RTT data. All other provisions set forth
in {{!RFC8253}} are unchanged, including connection initiation, message framing,
connection closure, certificate validation, peer identity, and failure handling.

# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Early Data

Early data (aka 0-RTT data) is a mechanism defined in TLS 1.3
{{I-D.ietf-tls-rfc8446bis}} that allows a client to send data ("early data")
as part of the first flight of messages to a server. Note that 
TLS 1.3 can be used without early data as per {{Section F.5 of I-D.ietf-tls-rfc8446bis}}. 
Infact, early data is permitted by TLS 1.3 only when the client and server 
share a Pre-Shared Key (PSK), either obtained
externally or via a previous handshake. The client uses the PSK to
authenticate the server and to encrypt the early data. 

As noted in {{Section 2.3 of I-D.ietf-tls-rfc8446bis}}, the security
properties for early data are weaker than those for subsequent TLS-protected
data. In particular, early data is not forward secret, and there is no
protection against the replay of early data between connections.
{{Appendix E.5 of I-D.ietf-tls-rfc8446bis}} requires applicaitons not
use early data without a profile that defines its use. This document
specifies that PCEPS implementations that support TLS 1.3 MUST NOT use early data.

# Cipher Suites

Implementations that support TLS 1.3 {{I-D.ietf-tls-rfc8446bis}} 
are REQUIRED to support the mandatory-to-implement cipher
suites listed in {{Section 9.1 of I-D.ietf-tls-rfc8446bis}}.

Implementations that support TLS 1.3 MAY implement additional TLS 
cipher suites that provide mutual authentication and confidentiality,
which are required for PCEP.

PCEPS Implementations SHOULD follow the recommendations given in
{{!I-D.ietf-uta-rfc7525bis}}.

~~~
So, this is what {{Section 9.1 of I-D.ietf-tls-rfc8446bis}} says:

  A TLS-compliant application MUST implement the TLS_AES_128_GCM_SHA256
  [GCM] cipher suite and SHOULD implement the TLS_AES_256_GCM_SHA384
  [GCM] and TLS_CHACHA20_POLY1305_SHA256 [RFC8439] cipher suites (see
  Appendix B.4).

  A TLS-compliant application MUST support digital signatures with
  rsa_pkcs1_sha256 (for certificates), rsa_pss_rsae_sha256 (for
  CertificateVerify and certificates), and ecdsa_secp256r1_sha256.  A
  TLS-compliant application MUST support key exchange with secp256r1
  (NIST P-256) and SHOULD support key exchange with X25519 [RFC7748].

Is there any reason to narrow the algorithm choices?

My guess is not.  These ought to be available in all TLS libraries.
~~~

# Security Considerations

The Security Considerations in TLS 1.3 are specified in {{I-D.ietf-tls-rfc8446bis}}.

The recommendations regarding Diffie-Hellman exponent reuse
are specified in {{Section 7.4 of I-D.ietf-uta-rfc7525bis}}.

The key Security Considerations for PCEP are described in {{RFC5440}}, {{?RFC8231}}, {{?RFC8281}}, and {{?RFC8283}}.

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

TLS 1.3 mutual authentication is used
to ensure that only authorized users and systems are able to send and receive PCEP messages. To this end, neither the PCC nor the PCE
should establish a PCEPS with TLS 1.3 connection with an unknown,
unexpected, or incorrectly identified peer; see {{Section 3.5 of RFC5440}}. If
deployments make use of a trusted list of Certification Authority (CA)
certificates {{!RFC5280}}, then the listed CAs should only issue certificates
to parties that are authorized to access the PCE. Doing otherwise
will allow certificates that were issued for other purposes to be
inappropriately accepted by a PCE.

The recommendations regarding certificate revocation checking
are specified in {{Section 7.5 of I-D.ietf-uta-rfc7525bis}}.


# IANA Considerations

There are no IANA considerations.

--- back

# Acknowledgments
{:numbered="false"}

We would like to thank Adrian Farrel for their review.
