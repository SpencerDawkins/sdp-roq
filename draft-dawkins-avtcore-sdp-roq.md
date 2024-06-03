---
title: SDP Offer/Answer for RTP over QUIC (RoQ)
abbrev: SDP O/A for RoQ
category: std

docname: draft-dawkins-avtcore-sdp-roq-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Web and Internet Transport"
workgroup: "Audio/Video Transport Core Maintenance"
keyword:
 - RTP over QUIC
 - RoQ
 - SDP
venue:
  group: "Audio/Video Transport Core Maintenance"
  type: "Working Group"
  mail: "avt@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/avt/"
  github: "SpencerDawkins/sdp-roq"
  latest: "https://SpencerDawkins.github.io/sdp-roq/draft-dawkins-avtcore-sdp-roq.html"

author:
 -
    fullname: "Spencer Dawkins"
    organization: Tencent America LLC
    country: United States of America
    email: spencerdawkins.ietf@gmail.com
 -
    fullname: Victor Pascual
    organization: Nokia
    city: Barcelona
    country: Spain
    email: victor.pascual_avila@nokia.com

normative:
  SDP-parameters:
    target: https://www.iana.org/assignments/sdp-parameters/sdp-parameters.xhtml#sdp-parameters-2
    title: "SDP Parameters - Proto"
    date: September 2021
  SDP-attribute-name:
    target: https://www.iana.org/assignments/sdp-parameters/sdp-parameters.xhtml#sdp-att-field
    title: "SDP Parameters - attribute-name"
    date: September 2021

informative:


--- abstract

This document describes these new SDP "proto" attribute values: "QUIC", "QUIC/RTP/SAVP", "QUIC/RTP/AVPF", and "QUIC/RTP/SAVPF", and describes how SDP Offer/Answer can be used to set up an RTP connection using QUIC as a transport protocol.

These proto values are necessary to allow the use of QUIC as an underlying transport protocol for applications such as SIP and WebRTC that commonly use SDP as a session signaling protocol to set up RTP connections.


--- middle

# Introduction {#intro}

This document describes these new SDP "proto" attribute values: "QUIC", "QUIC/RTP/SAVP", "QUIC/RTP/AVPF", and "QUIC/RTP/SAVPF", and describes how SDP Offer/Answer ({{!RFC3264}}) can be used to set up an RTP ({{!RFC3550}}) connection using QUIC ({{!RFC9000}} and related specifications).

These proto values are necessary to allow the use of QUIC as an underlying transport protocol for applications such as SIP ({{?RFC3261}}) and WebRTC ({{?RFC8825}}) that commonly use SDP as a session signaling protocol to set up RTP connections.

##Scope of this document {#scope}

This document focuses on the IANA registration and description of the RTP sessions using SDP Offer/Answer, as would be the case for many current RTP applications in common use, such as SIP ({{?RFC3261}}) and WebRTC ({{?RFC8825}}).

This document is intended as complementary to drafts such as {{!I-D.ietf-avtcore-rtp-over-quic}}, which largely focus on RTP/RTCP encapsulation in QUIC, so that the SDP experts can focus on SDP offer/answer aspects, and the RTP experts can focus on RTP/RTCP encapsulation aspects.

## Notes for Readers {#readernotes}

(Note to RFC Editor - if this document ever reaches you, please remove this section)

This document is intended for publication as a standards-track RFC in the IETF stream, but has not been adopted by any IETF working group, and does not carry any special status within the IETF.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Protocol identifiers {#idents-atts}

This document defines the following QUIC AVP profiles, in order to allow existing SIP and RTCWEB RTP applications to migrate more easily to QUIC:

 * RTP/AVPF ("Extended RTP Profile for Real-time Transport Control Protocol (RTCP)-Based Feedback (RTP/AVPF)"), as defined in {{!RFC4585}}.
  * RTP/SAVP ("The Secure Real-time Transport Protocol (SRTP)"), as defined in {{!RFC3711}}.
 * RTP/SAVPF ("Extended Secure RTP Profile for Real-time Transport Control Protocol (RTCP)-Based Feedback (RTP/SAVPF)"), as defined in {{!RFC5124}}.

As much as possible, attributes used in this section are reused from other specifications, with references to the original definitions.

## The QUIC proto {#quic}

The 'QUIC' protocol identifier is similar to the 'UDP' and 'TCP' protocol identifiers in that it only describes the transport protocol, and not the upper-layer protocol.

An 'm' line that specifies 'QUIC' MUST further qualify the application-layer protocol using an fmt identifier, such as "QUIC/RTP/AVPF".

Media described using an 'm' line containing the 'QUIC' protocol identifier are carried using QUIC streams, as defined in {{!RFC9000}}, or in QUIC DATAGRAMs, as defined in {{!RFC9221}}.

The following is an update to the ABNF for an 'm' line, as specified by {{!RFC8866}}, that defines a new value for the QUIC protocol.

~~~~~~
   media-field =         %s"m" "=" media SP port \["/" integer\]
                             SP proto 1*(SP fmt) CRLF

   m= line parameter        parameter value(s)
   ------------------------------------------------------------------
   <media>:                 (unchanged from {{!RFC8866}})
   <proto>:                 'QUIC'
   <port>:                  UDP port number
   <fmt>:                   (unchanged from {{!RFC8866}})
~~~~~~

## The QUIC/RTP/SAVP proto {#savp}

The following is an update to the ABNF for an 'm' line, as specified by {{!RFC8866}}, that defines a new value for the QUIC/RTP/SAVP protocol.

~~~~~~
   media-field =         %s"m" "=" media SP port \["/" integer\]
                             SP proto 1*(SP fmt) CRLF

   m= line parameter        parameter value(s)
   ------------------------------------------------------------------
   <media>:                 (unchanged from {{!RFC8866}})
   <proto>:                 'QUIC/RTP/SAVP'
   <port>:                  UDP port number
   <fmt>:                   (unchanged from {{!RFC8866}})
~~~~~~

## The QUIC/RTP/AVPF proto {#avpf}

The following is an update to the ABNF for an 'm' line, as specified by {{!RFC8866}}, that defines a new value for the QUIC/RTP/AVPF protocol.

~~~~~~
   media-field =         %s"m" "=" media SP port \["/" integer\]
                             SP proto 1*(SP fmt) CRLF

   m= line parameter        parameter value(s)
   ------------------------------------------------------------------
   <media>:                 (unchanged from {{!RFC8866}})
   <proto>:                 'QUIC/RTP/AVPF'
   <port>:                  UDP port number
   <fmt>:                   (unchanged from {{!RFC8866}})
~~~~~~

## The QUIC/RTP/SAVPF proto {#savpf}

The following is an update to the ABNF for an 'm' line, as specified by {{!RFC8866}}, that defines a new value for the QUIC/RTP/SAVPF protocol.

~~~~~~
   media-field =         %s"m" "=" media SP port \["/" integer\]
                             SP proto 1*(SP fmt) CRLF

   m= line parameter        parameter value(s)
   ------------------------------------------------------------------
   <media>:                 (unchanged from {{!RFC8866}})
   <proto>:                 'QUIC/RTP/SAVPF'
   <port>:                  UDP port number
   <fmt>:                   (unchanged from {{!RFC8866}})
~~~~~~

## A QUIC/RTP/AVPF Offer

A complete example of an SDP offer using QUIC/RTP/AVPF might look like:

|SDP line | Notes |
|v=0 |Same as {{!RFC8866}}|
|o=jdoe 3724394400 3724394405 IN IP4 198.51.100.1 |Same as {{!RFC8866}}|
|s=Call to John Smith |Same as {{!RFC8866}}|
|i=SDP Offer #1 |Same as {{!RFC8866}}|
|u=http://www.jdoe.example.com/home.html |Same as {{!RFC8866}}|
|e=Jane Doe <jane@jdoe.example.com> |Same as {{!RFC8866}}|
|p=+1 617 555-6011 |Same as {{!RFC8866}}|
|c=IN IP4 198.51.100.1 |Same as {{!RFC8866}}|
|t=0 0 |Same as {{!RFC8866}}|
|m=audio 49170 RTP/AVP 0 |Same as {{!RFC8866}}|
|m=audio 49180 RTP/AVP 0 |Same as {{!RFC8866}}|
|m=video 51372 QUIC/RTP/AVPF 99 |QUIC transport|
|a=setup:passive|will wait for QUIC handshake (setup attribute from {{!RFC4145}})|
|a=connection:new|don't want to reuse an existing QUIC connection (connection attribute from {{!RFC4145}})|
|c=IN IP6 2001:db8::2 |Same as {{!RFC8866}}|
|a=rtpmap:99 h266/90000 |H.266 VVC codec {{?I-D.ietf-avtcore-rtp-vvc}}|

This example is largely based on an example appearing in {{!RFC8866}}, Section 5, but is using QUIC/RTP/AVPF to support a newer codec.

Because QUIC uses connections for both streams and datagrams, we are reusing two session- and media-level SDP attributes from {{SDP-attribute-name}} that were defined in {{!RFC4145}} for use with TCP: setup and connection.

This example SDP offer might be included in a SIP Invite.

## Discussion

"QUIC", "QUIC/RTP/SAVP", "QUIC/RTP/AVPF", and "QUIC/RTP/SAVPF" are defined in the document now.

I'd like to minimize the number of protocol identifiers, based on identified needs rather than anticipated needs. Is it necessary to also define QUIC/RTP/AVP, or would that just be for completeness?

This document currently includes secure profiles for middlebox support of legacy RTP endpoints.
The thinking is that if a RoQ endpoint is communicating with a non-RoQ RTP endpoint using an RTP middlebox, the RoQ endpoint might reasonably use an insecure AVP profile when sending to the middlebox, but that endpoint doesn't have any way to control whether the RTP middlebox uses a secure profile when exchanging RTP with a non-RoQ endpoint.

RoQ allows the encapsulation of RTP in both QUIC streams and in QUIC DATAGRAMs. We will include "stream" and "dgram" variants of any profiles we define.

We've talked (a LOT) about including variants that failover to TCP/UDP/QUIC transport if UDP is blocked, so that an endpoint can proceed after ICE candidate probing, but can we assume that applications that can do other encapsulations will offer protocol identifiers for those encapsulations, in addition to "QUIC/RTP/AVPF" and related protocol identifiers?

# Opening a QUIC connection for use with RoQ

QUIC authentication with a "roq" ALPN or other ALPN, as described in Section 4.1 of draft-ietf-avtcore-rtp-over-quic

Things to remember for this section

* SDP "setup" attribute, defined in {{!RFC4145}} (a=setup:active, passive, actpass, holdconn)

* SDP "connection" attribute, defined in [RFC4145] (a=connection:new, existing)

* Check QUIC support of "rtcp-mux"

* Check QUIC impacts on BUNDLE

* Interaction with UDP-Connect to open pinholes in corporate proxies?

# Implications of using ICE with SDP from RFC 8842

Things to remember in this section

* SDP "fingerprint" attribute, defined in {{?RFC8122}}

* SDP "tls-id" attribute, defined in {{?RFC8842}}

* QUIC address validation and ICE candidate pairs

* QUIC Ping frames and ICE consent freshness (RFC 7675)# Interaction with Other RTP extensions

# Negotiating for specific QUIC feedback replacing AVP/AVPF feedback

 Things to remember in this section

* What we might say about RFC 4585 AVPF

* What we might say about RFC 5104 Codec Control Messages

* What we might say about RFC 8888 congestion control

# Considerations for end-to-end payload protection

 Things to remember in this section

* Technical solution - support SFRAME

* Human solution - require conformant RoQ middleboxes to bridge between RoQ and secure profiles (no RoQ=>AVP, no RoQ=>AVPF)

# Security Considerations

TODO Security

# IANA Considerations

TODO IANA Considerations from {{idents-atts}}.

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
