---
title: "Early Attestation is Broken"
category: info

docname: draft-usama-seat-early-attestation-is-broken-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Secure Evidence and Attestation Transport"
venue:
  group: "Secure Evidence and Attestation Transport"
  type: "Working Group"
  mail: "seat@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/seat"
  github: "muhammad-usama-sardar/seat-early-attestation-broken"
  latest: "https://muhammad-usama-sardar.github.io/seat-early-attestation-broken/draft-usama-seat-early-attestation-is-broken.html"

author:
 -
    fullname: "Muhammad Usama Sardar"
    organization: TU Dresden
    email: "muhammad_usama.sardar@tu-dresden.de"

normative:
  I-D.ietf-tls-rfc8446bis:

informative:
  I-D.fossati-seat-early-attestation: broken-proposal
  I-D.fossati-seat-expat:
  SEAT-Charter:
     title: "Secure Evidence and Attestation Transport (SEAT): Charter for Working Group"
     date: 2025,
     target: https://datatracker.ietf.org/wg/seat/about/
     author:
      - ins: IETF
  Key-Schedule:
     title: "Perspicuity of Attestation Mechanisms in Confidential Computing: Validation of TLS 1.3 Key Schedule"
     date: October 2025,
     target: https://www.researchgate.net/publication/396245726_Perspicuity_of_Attestation_Mechanisms_in_Confidential_Computing_Validation_of_TLS_13_Key_Schedule
     author:
      - ins: M. U. Sardar

--- abstract

Sheffer et al. published {{I-D.fossati-seat-early-attestation}} on 9th January, 2025
and despite being wildly out of scope of SEAT charter, the draft made its place -- getting
two-thirds of meeting time -- in the agenda for upcoming SEAT interim meeting within hours of
publishing. In comparison, our request to present {{I-D.fossati-seat-expat}} fully within the charter
was refused.
In this document, we disprove the claim made in {{I-D.fossati-seat-early-attestation}} for backward compatibility
with standard TLS {{I-D.ietf-tls-rfc8446bis}}. We argue that {{I-D.fossati-seat-expat}} is a
much more reaonsable way of achieving the goal within the scope of SEAT charter.


--- middle

# Introduction

We argue that:

* {{I-D.fossati-seat-early-attestation}} is out of scope of SEAT WG charter.
* Several claims in {{I-D.fossati-seat-early-attestation}} are broken.
Specifically, we prove that proposed key schedule is inconsistent with {{I-D.ietf-tls-rfc8446bis}}.
* {{I-D.fossati-seat-early-attestation}} breaks most -- if not all -- proofs done to date for TLS 1.3.



# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Out of Scope

{{SEAT-Charter}} has:

{:quote}
>  The attested (D)TLS protocol extension will not modify the (D)TLS
protocol itself. It may define (D)TLS extensions to support its goals
but will not modify, add, or remove any existing protocol messages
or modify the key schedule.

Contrary to the crystal clear statement of scope:

* {{Section 4.1 of -broken-proposal}} adds a new protocol message named "Attestation".
* {{Section 5.6 of -broken-proposal}} modifies the key schedule.

Both are subtle and error-prone. Such intesive changes should not bypass FATT process at TLS WG by any means.
SEAT has just a mention of formal analysis in its charter and no real process.
SEAT also does not have the blessing of many TLS experts. It makes pursuing
such a work in SEAT almost surely to lead to failure. We recommend the authors of
{{I-D.fossati-seat-early-attestation}} to submit the draft to TLS WG, where such
changes are in scope.

## Comparison with our draft

In comparison, {{I-D.fossati-seat-expat}} makes no changes to TLS and is fully in scope of SEAT charter.

# Broken Claims
Too many claims in {{I-D.fossati-seat-early-attestation}} are broken. We present one example which invalidates
most of other claims. The key schedule proposed in {{Section 5.6 of -broken-proposal}} is not consistent
with {{I-D.ietf-tls-rfc8446bis}}.


Using notations from {{Key-Schedule}}:

~~~~
hs = HKDF-Extract(salt1,gxy)
~~~~

whereas this draft proposes:

~~~~
hs' = HKDF-Extract(0,gxy)
~~~~


## Proof
{: #sec-proof }
Using definition of salt1 {{Key-Schedule}}:

~~~~
salt1 != 0
~~~~

Therefore, it comes that:

~~~~
hs != hs'
~~~~

Hence, the key schedule in {{I-D.fossati-seat-early-attestation}} is inconsistent with {{I-D.ietf-tls-rfc8446bis}}.

## Comparison with our draft

In comparison, {{I-D.fossati-seat-expat}} uses standard TLS key schedule without any changes.

# Breaking Formal Proofs

Because of above key schedule change, the draft breaks most -- if not all -- proofs done to date for TLS 1.3.

## Comparison with our draft

In comparison, we are making a careful effort to preserve security properties for our draft {{I-D.fossati-seat-expat}}.


# Security Considerations

This draft helps make this world more secure by refuting the security claims in {{I-D.fossati-seat-early-attestation}}
and by pushing against disruption of FATT process of TLS WG. Security is dependent on weakest link and we believe
{{I-D.fossati-seat-early-attestation}} is the weakest link in the security of TLS. Hence, we view post-handshake attestation
as the most appropriate option.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

We thank the authors of {{I-D.fossati-seat-early-attestation}} for putting together something,
which is already long overdue.

Since the proof in {{sec-proof}} is based on the working done in {{Key-Schedule}}, we thank
all those acknowledged there: namely Arto Niemi, Hannes Tschofenig, Thomas Fossati, Eric Rescorla,
and Ionut Mihalcea
