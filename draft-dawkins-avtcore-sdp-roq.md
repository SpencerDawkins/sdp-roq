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
    organization: Unaffiliated
    country: United States of America
    email: spencerdawkins.ietf@gmail.com
 -
    fullname: Victor Pascual
    organization: Nokia
    city: Barcelona
    country: Spain
    email: victor.pascual_avila@nokia.com

normative:
  SDP-protos:
    target: https://www.iana.org/assignments/sdp-parameters/sdp-parameters.xhtml#sdp-parameters-2
    title: "SDP Parameters - Proto"
    date: September 2021
  SDP-attribute-name:
    target: https://www.iana.org/assignments/sdp-parameters/sdp-parameters.xhtml#sdp-att-field
    title: "SDP Parameters - attribute-name"
    date: September 2021

informative:

--- abstract

This document describes several new SDP "proto" and "attribute-name" attribute values in the "Session Description Protocol (SDP) Parameters" IANA registry that can be used to describe QUIC transport for RTP and RTCP packets, and describes how SDP Offer/Answer can be used to set up an RTP connection using QUIC as a transport protocol.

These new values are necessary to allow the use of QUIC as an underlying transport protocol for applications such as SIP and WebRTC that commonly use SDP as a session signaling protocol to set up RTP connections.

--- middle

# Introduction {#intro}

This document describes several new SDP "proto" and "attribute-name" attribute values in the "Session Description Protocol (SDP) Parameters" IANA registry ({{SDP-protos}} and {{SDP-attribute-name}}) that can be used to describe QUIC transport for RTP and RTCP packets, and describes how SDP Offer/Answer ({{!RFC3264}}) can be used to set up an RTP ({{!RFC3550}}) connection using QUIC ({{!RFC9000}} and related specifications), as defined in {{!I-D.ietf-avtcore-rtp-over-quic}}.

These new values are necessary to allow the use of QUIC as an underlying transport protocol for applications such as SIP ({{?RFC3261}}) and WebRTC ({{?RFC8825}}) that commonly use SDP as a session signaling protocol to set up RTP connections.

##Scope of this document {#scope}

This document focuses on the IANA registration and description of the RTP sessions using SDP Offer/Answer, as would be the case for many current RTP applications in common use, such as SIP ({{?RFC3261}}) and WebRTC ({{?RFC8825}}).

## Notes for Readers {#readernotes}

(Note to RFC Editor - if this document ever reaches you, please remove this section)

This document is intended for publication as a standards-track RFC in the IETF stream, but thus far has not been adopted by any IETF working group, so does not carry any special status within the IETF.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

Because the use of SDP to describe RTP over QUIC transport relies heavily on terminology introduced in {{!I-D.ietf-avtcore-rtp-over-quic}}, the definitions in that document are prerequisite for understanding this document, and those terms are included here by reference.

# New SDP Protocol identifiers {#idents-atts}

This document reuses the following AVP profiles from {{SDP-protos}}, in order to allow existing SIP and RTCWEB RTP applications to migrate more easily to RTP over QUIC:

 * RTP/AVP ("RTP Profile for Audio and Video Conferences with Minimal Control"), as defined in {{!RFC3551}}.
 * RTP/AVPF ("Extended RTP Profile for Real-time Transport Control Protocol (RTCP)-Based Feedback (RTP/AVPF)"), as defined in {{!RFC4585}}.
 * RTP/SAVP ("The Secure Real-time Transport Protocol (SRTP)"), as defined in {{!RFC3711}}.
 * RTP/SAVPF ("Extended Secure RTP Profile for Real-time Transport Control Protocol (RTCP)-Based Feedback (RTP/SAVPF)"), as defined in {{!RFC5124}}.

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

## RoQ RTP Protos {#rtp-protos}

As much as possible, attributes used in this section are reused from other specifications, with references to the original definitions.

### The QUIC/RTP/AVP proto {#avp}

The following is an update to the ABNF for an 'm' line, as specified by {{!RFC8866}}, that defines a new value for the QUIC/RTP/AVP protocol.

~~~~~~
   media-field =         %s"m" "=" media SP port \["/" integer\]
                             SP proto 1*(SP fmt) CRLF

   m= line parameter        parameter value(s)
   ------------------------------------------------------------------
   <media>:                 (unchanged from {{!RFC8866}})
   <proto>:                 'QUIC/RTP/AVP'
   <port>:                  UDP port number
   <fmt>:                   (unchanged from {{!RFC8866}})
~~~~~~

### The QUIC/RTP/AVPF proto {#avpf}

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

### The QUIC/RTP/SAVP proto {#savp}

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

### The QUIC/RTP/SAVPF proto {#savpf}

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

# New SDP Attribute-Names

## RoQ QUIC-DATAGRAMs Attribute {#quic-datagrams}

As noted in {{!I-D.ietf-avtcore-rtp-over-quic}}, the RoQ specification only assumes a baseline QUIC implementation as defined in {{!RFC8999}}, {{!RFC9000}}, {{!RFC9001}}, and {{!RFC9002}}, and this baseline does not provide unreliable datagrams, which are defined in {{!RFC9221}}.

It is very likely that RoQ implementers will wish to use QUIC DATAGRAMs, for a variety of reasons too large to list in this specification.

Actual support for QUIC DATAGRAMs is negotiated between two QUIC endpoints, as described in Section 3 of {{!RFC9221}}, and nothing specified in SDP will cause a QUIC endpoint that does not advertise support for QUIC DATAGRAMs to suddenly begin to support them. However, it may be useful to tell a RoQ receiver that the RoQ sender plans to send QUIC DATAGRAMs, and to allow a RoQ receiver to tell the SDP sender that the RoQ receiver does not plan to support QUIC DATAGRAMs for that media flow.

In order to provide this capability, this section defines a new SDP media-level attribute, "quic-datagrams". The attribute can be associated with an SDP media description ("m=" line) with any of the QUIC proto values defined in {{quic}}.

If the quic-datagrams attribute is present, the RoQ sender indicates its intention to use QUIC DATAGRAMs for the associated media flow, and the RoQ receiver indicates its willingness to accept QUIC DATAGRAMs for that media flow.

The definition of the SDP "quic-datagrams" attribute is:

Attribute name:  quic-datagrams

Type of attribute:  media

Mux category:  IDENTICAL

> **Author's Note:** This specification sets the mux category (as discussed in Section 4 of {{?RFC8859}}) as IDENTICAL, as an RTP mixer which is multiplexing several incoming streams onto one connection needs to provide the same quidance to a RoQ receiver for all multiplexed media flows.

Subject to charset:  No

Purpose:  This attribute provides a hint as to whether the media associated with the SDP media description is likely to arrive via QUIC DATAGRAMs. It is a property attribute, which does not take a value.

Contact name:  Spencer Dawkins

Contact e-mail:  spencerdawkins.ietf@gmail.com

Reference:  {{!I-D.dawkins-avtcore-sdp-roq}} (This document)

Syntax:

~~~~~~
    quic-datagrams
~~~~~~

## RoQ Flow Identifiers {#rtp-quic-flow-id}

Section 5.1 of {{!I-D.ietf-avtcore-rtp-over-quic}} introduces a multiplexing identifier for RTP flows carried over a QUIC connection called "Flow Identifiers". This section defines a new SDP media-level attribute, "roq-flow-id". The attribute can be associated with an SDP media description ("m=" line) with any of the QUIC proto values defined in {{quic}}. In that case, the "m=" line port value indicates the port of the underlying QUIC transport UDP port, and the "roq-flow-id" value indicates the RoQ Flow Identifier.

No default value is defined for the SDP "roq-flow-id" attribute. Therefore, if the attribute is not present, the associated "m=" line MUST be considered invalid.

The definition of the SDP "roq-flow-id" attribute is:

Attribute name:  roq-flow-id

Type of attribute:  media

Mux category:  CAUTION

> **Author's Note:** This specification sets the mux category (as discussed in Section 4 of {{?RFC8859}}) as CAUTION, as an RTP mixer which is multiplexing several incoming streams onto one connection needs to ensure that RoQ Flow Identifiers do not overlap, and might need to rewrite the Flow Identifiers in received streams when further multiplexing them.

Subject to charset:  No

Purpose:  This attribute indicates the RoQ Flow Idenfitier associated with the SDP media description.

Contact name:  Spencer Dawkins

Contact e-mail:  spencerdawkins.ietf@gmail.com

Reference:  {{!I-D.dawkins-avtcore-sdp-roq}} (This document)

Syntax:

~~~~~~
    roq-flow-id = 1*19(DIGIT) ; DIGIT defined in RFC 4566
~~~~~~

The RoQ flow identifier range is between 0 and 4611686018427387903 (2^62 - 1) (both included). Leading zeroes MUST NOT be used.

# Interaction with Existing RTP extensions

* {{!I-D.ietf-avtcore-rtp-over-quic}} defines how RTP and RTCP can be multiplexed onto a single QUIC connection. An application that will perform this multiplexing uses the "rtcp-mux" attribute defined in {{!RFC5761}} in its SDP signaling.

# Work in Progress

**NOTE:** This section is still under construction. The contents are likely to change significantly before settling down!

"QUIC", "QUIC/RTP/AVP", "QUIC/RTP/AVPF", "QUIC/RTP/SAVP", and "QUIC/RTP/SAVPF" are defined in the document now.

## Opening a QUIC connection for use with RoQ

This document assumes that an authenticated QUIC connection will be opened using a "roq" ALPN or some other ALPN, as described in Section 4.1 of {{!I-D.ietf-avtcore-rtp-over-quic}}.

The SDP "setup" attribute, defined for media over TCP in {{!RFC4145}}, will be reused to indicate which endpoint initiates a QUIC connection (whether the endpoint actively opens a QUIC connection, or accepts an incoming QUIC connection.

The SDP "connection" attribute, defined for TCP in [RFC4145], will be reused to indicate whether the endpoint will open a new QUIC connection, or reuse an existing QUIC connection.

* Check QUIC impacts on BUNDLE

* Interaction with UDP-Connect to open pinholes in corporate proxies?

## Implications of using ICE with SDP from RFC 8842

Things to remember in this section

* SDP "fingerprint" attribute, defined in {{?RFC8122}}

* SDP "tls-id" attribute, defined in {{?RFC8842}}

* QUIC address validation and ICE candidate pairs

* QUIC Ping frames and ICE consent freshness (RFC 7675)

## Negotiating for specific QUIC feedback replacing AVP/AVPF feedback

 Things to remember in this section

* What we might say about RFC 4585 AVPF

* What we might say about RFC 5104 Codec Control Messages

* What we might say about RFC 8888 congestion control

# A QUIC/RTP/AVPF Offer Example

**Editor's Note:** I still need to clean this example up, after discussion on the rest of the document.

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
|a=quic-datagrams | Expects to use QUIC DATAGRAMs for this audio media stream, as defined in this specification |
|m=audio 49180 RTP/AVP 0 |Same as {{!RFC8866}}|
|a=quic-datagrams | Expects to use QUIC DATAGRAMs for this audio media stream, as defined in this specification |
|m=video 51372 QUIC/RTP/AVPF 99 |QUIC transport|
|a=setup:passive|will wait for QUIC handshake (setup attribute from {{!RFC4145}})|
|a=connection:new|don't want to reuse an existing QUIC connection (connection attribute from {{!RFC4145}})|
|a=roq-flow-id:2 | RoQ Flow Identifier shall be 2 for streams described by this SDP media description|
|c=IN IP6 2001:db8::2 |Same as {{!RFC8866}}|
|a=rtpmap:99 h266/90000 |H.266 VVC codec {{?I-D.ietf-avtcore-rtp-vvc}}|

This example is largely based on an example appearing in {{!RFC8866}}, Section 5, but is using QUIC/RTP/AVPF to support a newer codec.

Because QUIC uses connections for both streams and datagrams, we are reusing two session- and media-level SDP attributes from {{SDP-attribute-name}} that were defined in {{!RFC4145}} for use with TCP: setup and connection.

This example SDP offer might be included in a SIP Invite.

# Security Considerations

The security considerations sections of the Normative References in this document are incorporated by reference.

This document currently includes secure profiles for middlebox support of legacy RTP endpoints.
The thinking is that if a RoQ endpoint is communicating with a non-RoQ RTP endpoint using an RTP middlebox, the RoQ endpoint might reasonably use an insecure AVP profile when sending to the middlebox, but that endpoint doesn't have any way to control whether the RTP middlebox uses a secure profile when exchanging RTP with a non-RoQ endpoint.

* Technical solution - support SFRAME

* Human solution - require conformant RoQ middleboxes to bridge between RoQ and secure profiles (no RoQ=>AVP, no RoQ=>AVPF)

# IANA Considerations

This document defines new IANA values in the {{SDP-protos}} and {{SDP-attribute-name}} registries.

## quic-datagrams {#IANA-quic-datagrams}

This document defines a new SDP media-level attribute, "quic-datagrams". The details of the attribute are defined in {{quic-datagrams}}.

## roq-flow-id

This document defines a new SDP media-level attribute, "roq-flow-id". The details of the attribute are defined in {{rtp-quic-flow-id}}.

--- back

# Acknowledgments
{:numbered="false"}

The authors thank Sam Hurst for sharing his thoughts about the challenges of developing SDP for RoQ, and for providing specific comments and draft text.

The authors thank Mathis Engelbart for helping to keep this draft aligned with {{!I-D.ietf-avtcore-rtp-over-quic}}.
