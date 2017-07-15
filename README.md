# tinfoil: TLS Is Not For Obligatory Interception, Luckily

This repository is a place to collect together arguments for not breaking TLS.
By "breaking TLS" I mean significantly weakening the protocol, or
implementations or deployments of the protocol, so that the security claims
made in the protocol specification can no longer be credibly maintained.

Note that one does not need to accept all arguments in order to properly
conclude that breaking TLS is a bad plan or that some specific break-TLS
proposal is a bad plan. One killer argument per proposal should be enough, but
it's useful to collect more than that anyway.

At some point it may be worth turning this into an Internet-draft to save us
all time for when the next break-TLS proposal comes along.

PRs that add to (really, record) those arguments are welcome, especially if
they identify problems with specific proposals to break TLS.

## Scheme-independent Points

In this section I call out some generic issues that affect all "break-TLS"
proposals:

1. With all break-TLS attempts so far, the proponents only seem to have analysed
  the deployments or use-cases about which they care. As far as one can see
they appear to generally ignore other uses of TLS. But there are many many uses
of TLS, for example the TLS1.2 RFC is currently (20170711) [referenced
by](https://datatracker.ietf.org/doc/rfc5246/referencedby/) 434 other IETF
specifications, and is
[cited](https://scholar.google.com/scholar?q=http%3A%2F%2Fwww.hjp.at%2Fdoc%2Frfc%2Frfc5246.html&btnG=&hl=en&as_sdt=0%2C5)
by 3,281 publications in Google Scholar.  Accepting any proposal that might
weaken such an important security  protocol without due dilligence is utterly
unwise.  And it is hard to see how anyone could really do that due dilligence
given the breadth of use of TLS today - both in openly specified foo/TLS
applications but also in the presumably larger number of not publicly
documented applications using TLS.

1. Note that I make no allegations about the bona-fides of any of the proponents
  of the "break-TLS" schemes.  I know and respect some of them, but consider
them misguided in contributing to proposals to break TLS.  However, we cannot
ignore the fact that some governments are very keen to weaken Internet security
and privacy and have allocated significant budgets for that.
[BULLRUN](https://www.theguardian.com/world/2013/sep/05/nsa-gchq-encryption-codes-security)
for example is reported to have involved wasting/spending US$250M/yr on this.
It is inevitable that some of that money ends up being spent/wasted on schemes
to break or weaken TLS.  The consequence of that is that it is entirely proper
to consider the [Pervasive Monitoring](https://tools.ietf.org/html/rfc7258)
aspects of any proposal as part of our [threat
model](https://tools.ietf.org/html/rfc7624) no matter what motivations are set
out by proponents. (And again, considering this says nothing at all proponents'
motivations, it's just a thing that we have to consider regardless.)

1. TLS is already hard, as we have seen from two decades of exploits against
  implementations and, occasionally, against the protocol itself. The added
complexity of any "break TLS" proposal makes that situation worse, and
logically must do so. But we have more than argument to go on here - there is
evidence for this from peer-reviewed academic studies and examination of TLS
interception implmentations, for example [Durumeric et
al](https://www.internetsociety.org/sites/default/files/ndss2017_04A-4_Durumeric_paper.pdf)
found that "that nearly all [interception proxies] reduce connection security
and many introduce severe vulnerabilities" whilst [de Carnavalet et
al](http://users.encs.concordia.ca/~mmannan/publications/ssl-interception-ndss2016.pdf)
performed a "systematic analysis uncovered that several of these tools severely
affect TLS security on their host machines." So, where there is evidence
publicly available, it seems to indicate that additional complexity (via
proxies or other methods of breaking TLS) reduces security. To my knowledge,
none of the proposed "break TLS" schemes has ever offered evidence that they
would lead to an overall improvement in security.

1. In many cases like this people also argue that even if breaking TLS is
  undesirable, it's better to do that undesirable thing openly inside the IETF
as it would happen elsewhere in any case and perhaps be done worse. Without
specific evidence that organisation foo is about to take actionn bar, that
argument is impossible to judge, so is at best neutral. When there is specific
evidence of something relevant being done elsewhere, then the IAB and IESG have
liaison mechaisms that can be used for the purpose for which they were
designed.  IOW, "if we don't, they will" is not a good argument that the IETF
should do any particular thing at all. In the author's experience, this
argument is commonly assoociated with proponents of proposals that might be fairly
described as being engaged in forum shopping. In the case of "break TLS"
proposals, one could argue (and I would) that the reason TLS is widely
used is, in significant part, because TLS is not broken. 

1. In 2014, the Internet Architecture Board issued a 
[statement](https://www.iab.org/2014/11/14/iab-statement-on-internet-confidentiality/)
to the effect that "Internet depended on users having confidence that the network would protect 
their private information" and that we should "make encryption the norm for Internet traffic." 
All proposals to break or weaken TLS go against that sound advice (not repeated
here because we'd end up repeating almost all of it). 
The IAB statement 
recognises that increasingly widespread use of TLS causes issues
for network operations, but no response that amounts to attempting to break TLS
so far presented can be jusified due to the huge potential cost and impact
of breaking TLS. 

1. Most or all approaches to breaking TLS seem to involve changing TLS from a
two-party protocol (ignoring CAs for now) into a multi-party protocol, but
one that mimics the behavior of TLS in order to have a chance to get deployed
on the Internet.  A multi-party transport security protocol however would
require either a substantially different API or to move up the stack to solve
whatever problem one is facing.  

## Specific "break-TLS" proposals

### [draft-green-tls-static-dh-in-tls13-01](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01)

This is the current (July 2017) proposal for breaking TLS.
It fails in the following ways:

1. The [TLS working group charter](https://tools.ietf.org/wg/tls/charters)
dated 2017-03-30 calls for improving the security of TLS, and 
this proposal involves leaking private key material and hence
clearly falls outside the charter.

1. Stephen Checkoway [raised](https://www.ietf.org/mail-archive/web/tls/current/msg23913.html) a different charter issue:
"There are essentially two separate proposals in the I-D. Section 5 proposes a slight change to TLS that results in no changes on the wire and, as far as I can tell, is already allowed (but should probably be discouraged) in the TLS 1.3 I-D. Thus, there's nothing for the WG to do.
Sections 6 and 7 propose a new protocol for distributing key pairs. The use case is TLS, but it isn't specific to TLS and doesn't interact with TLS (outside of using TLS for HTTPS). As such, I believe it's outside the WG's charter."

1. TLS1.3 (and DTLS1.3) are still not finished and any
work within the IETF on this draft will put those efforts
at risk. For example, the abstract of the latest
[TLS1.3 draft](https://tools.ietf.org/html/draft-ietf-tls-tls13-21)
says that TLS is "designed to prevent eavesdropping" - changes
to that and possibly many other claims would be needed
if the TLS working group adopted this, or any similar, draft.
Those changes would not all be editorial and would likely
be time-consuming.

1. As a result of the discussion of this draft, Christian Huitema has proposed a
[pull request](https://github.com/tlswg/tls13-spec/pull/1049) for TLS1.3 to
encourage implementers to effectively make the idea in this draft harder to deploy,
by documenting the core static DH value idea as an attack against which
TLS clients ought attempt to defend themselves.
Adopting work on static DH values would therefore have the effect of forcing
the working group to work against itself, further delaying and putting TLS1.3
at risk.

1. TLS1.3 has undergone significant academic and other analyses, 
(including two academic workshops,
[TRON](https://www.internetsociety.org/events/ndss-symposium-2016/tls-13-ready-or-not-tron-workshop-programme)
in 2016, and [TLS-DLV](https://www.mitls.org/tls:div/) in 2017), 
none of which
  (to my knowledge) envisaged the use of static DH values, nor of centrally
generated DH values being "pushed" to many TLS servers. Similarly, work on the
TLS application layer PDU protection didn't envisge re-use of DH values in
possibly many TLS sessions.  Adoption of any work in this space could require
re-doing some of the analysis work, possibly delaying TLS1.3 or (worse)
resulting in new problems being found after widespread deployment. Given that
this draft would create new active and passive attack possibilities, such new
analysis work would seem to be required, especially if proprietary 
extensions to the proposed "TSK" protocol affect the cryptogrphy used
in TLS sessions. 

	- The work done by academic analyses of TLS1.3 has been valuable in terms
of improving the quality of the IETF's output. Radical changes to TLS1.3 at this point 
will likely disincent researchers from contributing as a part of the process 
in future as they may perceive the IETF to be moving the goalposts at the
last minute, indicating that IETF participants do not value their inputs.
(I admit that is speculative, but it's based on some previous discussions on the WG 
list - it'd be good to get feedback from researchers to check.)

1. There could be similar problems caused for the QUIC protocol development
  work, as that relies upon TLS1.3 and has similar design elements that
could be perturbed if static DH private values were used. And QUIC has recently
been
[proposed](https://www.ietf.org/mail-archive/web/quic/current/msg01878.html) as
a substrate for 5G efforts in another SDO with fairly aggressive timelines, so
delays in developing QUIC caused by breaking TLS could have significant effects
elsewhere.

1. This draft aims/claims to enable key-leaking/wiretapping only "within"
enterprise networks, but there is no way (and cannot be a way)
to constrain the use of this scheme to such (parts of) networks.
Figure 3 of the draft clearly describes a generic 
key-leaking/wiretapping architecture for TLS that could (and would)
be used in many other circumstances that the authors
have apparently not envisaged.

1. We also have a documented case where a law enforcement agency 
has attempted to coerce a mail service provider 
called [Lavabit](https://en.wikipedia.org/wiki/Lavabit) into
providing TLS private key materials. Developing an API
such as envisaged in this draft would encourage such
law enforcement and other agency attempts at key recovery in many countries.
So the unintended uses for this are as real as the intended 
ones. (An aspect that is ignored by the draft and it's
proponents.)

1. This draft is targeted as being an IETF standards track document
but is a wiretapping scheme (or enables wiretapping) that meets the definition in
[RFC2804](https://tools.ietf.org/html/rfc2804) and therefore
cannot be a standards-track work item in the IETF.

	- Some may argue that even if this cannot be an
IETF standards track specification, it should still be
further developed by the IETF. IMO, (though this is
guessing to an extent) that also goes against 
[RFC2804](https://tools.ietf.org/html/rfc2804) 
which calls for documentation, and not development,
of deployed wiretapping schemes. Documenting the
wiretapping schemes that exist is a good thing. 
Developing new ones is a bad thing, for all the
reasons set out explicitly in RFC2804. As a 
speculative new wiretapping design, this draft
should not be an IETF specification of any kind, 
so long as RFC2804 has not been obsoleted, and
yet the authors here are making no attempt to
obsolete RFC2804, and are thus attempting an end-run
around IETF processes.

	- To my knowledge, all wiretapping schemes 
docuemted in RFC so far have been in the independent
stream (hence not IETF docuemts) or perhaps pre-date
RFC streams.

	- See below for more on this as a wiretapping
proposal.

1. This proposal entirely suits what governments doing
pervasive monitoring would need in an API. The IETF
has documented its consensus that [Pervasive Monitoring is an Attack](https://tools.ietf.org/html/rfc7258)
in RFC 7258, and this API would assist with such 
attacks. In case this seems somewhat abstract, we
have evidence of exactly this kind of key exfiltration
API in IPsec, as documented by [Wouters](https://nohats.ca/wordpress/blog/2014/12/29/dont-stop-using-ipsec-just-yet/)
and [Der Spiegel](http://www.spiegel.de/media/media-35515.pdf).
In that case, a collector has ciphertext packets and calls
an funcftion (passing in initial packets) to get a key for 
packet decryption. An implementation of such a function
could clearly benefit from the GET DH value API 
defined in this document. 

1. This draft tries to standardise broken crypto - forward
secrecy is a goal of cryptographic protocols and this 
draft deliberately aims to not provide forward secrecy
and indeed to break forward secrecy.  [BCP200](https://datatracker.ietf.org/doc/rfc1984/)
(which was promoted to BCP in 2015, and so is very
recent) specifically argues against such schemes:

	- "Escrow mechanisms inevitably weaken the security of the overall
   cryptographic system, by creating new points of vulnerability that
   can and will be attacked." 

	- "KEYS SHOULD NOT BE REVEALABLE
   The security of a modern cryptosystem rests entirely on the secrecy
   of the keys.  Accordingly, it is a major principle of system design
   that to the extent possible, secret keys should never leave their
   user's secure environment.  Key escrow implies that keys must be
   disclosed in some fashion, a flat-out contradiction of this
   principle.  Any such disclosure weakens the total security of the
   system."

	- "Even if escrowed encryption schemes are used, there is nothing to
   prevent someone from using another encryption scheme first.
   Certainly, any serious malefactors would do this; the outer
   encryption layer, which would use an escrowed scheme, would be used
   to divert suspicion."

	- "A major threat to users of cryptographic systems is the theft of
   long-term keys (perhaps by a hacker), either before or after a
   sensitive conversation.  To counter this threat, schemes with
   "perfect forward secrecy" are often employed.  If PFS is used, the
   attacker must be in control of the machine during the actual
   conversation.  But PFS is generally incompatible with schemes
   involving escrow of private keys.  (This is an oversimplification,
   but a full analysis would be too lengthy for this document.)"

	- Note that while the concept of key escrow that was being
	promulgated in the mid 1990's is not identical to that 
    being proposed here, the same vulnerabilities are
	created once one leaks private key materials, especially
	via a "standard" interface. 

1. The argument has been made that it would be better
to scrutinise proposals such as this openly in the IETF
instead of having individual vendors develop their
own bad-crypto implementations. As a counter to that,
while scrutiny is good for good crypto, the overall
ecosystem will be harmed by wide-spread use of the
same bad-crypto, so therefore in the case of
bad-crypto proposals such as this, it is better for
us all that there is not a single bad-crypto standard.

1. Ironically, the first author of [draft-green](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01)
is also a co-author of [keys under doormats](https://dspace.mit.edu/openaccess-disseminate/1721.1/102271).
While that latter report mainly considers government
mandated back-doors, it does 
also correctly point out that:
"Communications technologies designed to comply with government requirements for
backdoors for legal access have turned out to be insecure. For ten months in 2004 and
2005, 100 senior members of the Greek government (including the Prime Minister, the
head of the Ministry of National Defense and the head of the Ministry of Justice) were
wiretapped by unknown parties through lawful access built into a telephone switch owned
by Vodafone Greece [19]. In 2010 an IBM researcher observed that a Cisco architecture
for enabling lawful interception in IP networks was insecure. 2 This architecture had
been public for several years, and insecure versions had been implemented by several
carriers in Europe [20]. And when the NSA examined telephone switches built to comply
with government-mandated access for wiretapping, it discovered security problems with
all the switches submitted for testing[21]. Embedding exceptional access requirements
into communications technology will ensure even more such problems, putting not only
private-sector systems, but government ones, at risk."
The draft being discussed here is just enabling another form
of exceptional access, despite being designed for
enterprise-access and not for government access, so the fine points
quoted above are as applicable. 

	- One of the reactions to excessive government data retention has been to
	  suggest requiring corporations to store the relevant (or all!) data and
to then make that information available to government as needed.  The API
defined here could assist that kind of privatisation of government pervasive
monitoring as a kind of federated government backdoor, if governments mandated
use of this scheme.

1. A [suggestion](https://www.ietf.org/mail-archive/web/tls/current/msg23802.html)
was made on the TLS list that the deployment
of this scheme be made part of a "website's 
terms of service." Hiding the fact that one is
breaking TLS in legalese seems like a terrible
idea to this user of the web.

1. Whether or not use of this scheme is visible to the Internet 
has a number of consequences, all of which appear to be bad 
outcomes. 

	- This scheme doesn't allow normal TLS clients (without visibility of DH
	  re-use) and applications above or behind the TLS server to detect that
wiretapping is occurring. That means we cannot know if their threat models
match reality or not. That is essentially a layering issue - this, and all,
"break TLS" proposals change the semantics of TLS without letting the
applications and prtoocols that depend on TLS know that that change has
happened.

	- To this reader, it is not clear whether or not deployments of the -01
	  draft, would be detectable or not, from the Internet.  (Detection could
be visible to an "ordinary" client or might require a
[zmap-like](https://zmap.io/) survey.) It is clear though that -01 has enough
extension points so that an undetectable version of this scheme would be easy
to invent and deploy.

		- A strawman undetectable version: change the CMS wrapper to indicate that the
value contained therein is the RNG seed for a specific algorithm for
generating a sequence of DH private values. Other than e.g. one new OID,
that could look precisely the same as the current scheme. That should work
fine for any situation where the TLS server and the TLS decrypter have
similar CPU capabilities for the relevant traffic, which is probably many cases.

		- The upshot is: we cannot assume that this scheme will be
detectable.

	- If use of this scheme is detectable,and if it is not widely used, it will
	  likely fall out of use and end up worthless as the reputational cost of
being shown to be a deployment of this would be high. ("Hey, all my cookies are
leaked when I talk to example.com!") That means effort to scrutinise this in
the IETF would be wasted.
	
	- If this is detectable and somehow is widely deployed,  the impact of
	  exploits of the inevitable vulnerabilities would be gigantic as many many
TLS servers would offer an API to extract their DH private values.
("Hey, what domains TLS cleartext would you like to buy?")

	- If the scheme is initially detectable, and not very widely deployed
	  (highly likely at least at first), then deployments will make
modifications to the scheme to avoid detection. Such proprietary
modifications mean that all effort to "improve" or "scrutinise" this scheme
would certainly be wasted. Proprietary modifications could also significantly
weaken security.

1. Developing this interface within the IETF would arguably increase the
  probability of "worse solutions" being deployed as those would be easier to
integrate thanks to the existence of the "GET private key" API that TLS
servers might then offer. An argument for this draft was
[offered](https://www.ietf.org/mail-archive/web/tls/current/msg23817.html) that
standardising it would avoid such "worse solutions" - I see no evidence offered
and claim (with the same evidence;-) that the opposite would eventuate.

1. This scheme enables active attacks - once the DH private values are known,
  then all session keys are known and seemingly valid packets can be injected
into ongoing sessions. The abstract of the -01 draft is extermely misleading in
claiming that it is for "passive monitoring." No mention is made in the draft
of the active attacks it enables.

1. If this scheme were widespread, it would be "accidentally" deployed in places
  for which it was not intended. (See also
[RFC2804](https://tools.ietf.org/html/rfc2804).) The result could be active
attacks on e.g. widely downloaded web resources enabling active attacks on
browsers.  I would expect another decade of academic publications and real
exploits on TLS to result.

1. Applications built on TLS, e.g. the Web, would need to revise their security
  models to take into account this scheme, as it changes the threat model on
which they have been built.

1. For the enterprise uses claimed to justify this, there is no need for
  Internet-scale interoperability as the enterprise network is by-definition
under a single entity's control. (And if they cannot control their network,
then adding broken crypto seems even more unwise.)

1. Ossification: this draft would introduce new ways of ossifying TLS, for
  example, the so-called "TLS decrypter" would have to be able to handle all
updated ciphersuites before those could be used by bona-fide TLS clients and
servers.  We have seen that non-updated PKCS#1.5 crypto hardware has caused
problems with updating the crypto in TLS for decades now, and this would cause
similar problems.

1. Brian Carpenter points out (private communication, quoted with permission)
  that "server load balancing and TLS load balancing don't mix well. (See
[RFC7098](https://tools.ietf.org/html/rfc7098) for more.) In fact, there's no
practical alternative for large server farms except TLS proxying in front of
the servers.  At a quick glance it seems to me that draft-green doesn't
separate this issue from the rest. But server farms have no choice about
solving that problem, or the intrusion/DOS detection problem, but they do have
a choice about monitoring as such.  Separating the operational requirements
from the political requirements might be a good first step."

While I'm reluctant to comment on the details of a bad
design, there are a couple of things to point out: 

1. The fact that this draft both breaks and depends
upon TLS (in it's use of HTTP/TLS via RFC2818) means
that this drafts enables "meta-attacks" where another
party can eavesdrop on the connections between the
various TLS-breaking components defined here, if (as
would seem likely to me) the OPTIONAL additional CMS 
protection is not used. Similarly,
such a meta-attacker could likely inject chosen DH
values into a deployment. 

- [Section 7.2](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01#section-7.2)
defines a key request interface, but defines no authorisation
scheme, no HTTP authentiction, and requires no additional CMS
protection.
This creates an excellent target to attempt to find
when one has breached some network. That is an excellent 
example of the kind of additional vulnerability 
created by these schemes called out in BCP200 and 
[RFC2804](https://tools.ietf.org/html/rfc2804).
There is ample recent evidence [Wannacry](https://en.wikipedia.org/wiki/WannaCry_ransomware_attack)
that many enterprises are not capable of shutting down
or controlling such interfaces that either ought not be
offered or need to be protected.

- Richard Barnes [points out](https://www.ietf.org/mail-archive/web/tls/current/msg23793.html)
that there may be "unstated requirements here to support for resumption
(without having to find the previous session) and probably 0xRTT, and those
modes use in a shared secret in addition to the DH secret.  So you would need
to exfiltrate the PSK as well." If correct, that would seem to 
indicate that this proposal is broken. 

- The use of CMS means that there are already a vast array of possible (even
  deployment-speciic) extension points in this protocol. That means that it is
not possible to have any confidence that what would be designed is what would
be deployed. In fact, given the detectability issues above, it seems very
likely that non-deection (meaning non-compliance with the putative RFC) will be
a goal for implementers.

IMO none of the above ought not be fixed by IETF participants collectively.

#### Why is this wiretapping?

Some IETF TLS WG list participants have denied that this is a wiretapping
propospal, based on it somehow not matching the definitions in 
[RFC2804](https://tools.ietf.org/html/rfc2804).
Firstly, that is far too lawyerly and IMO being overly pedantic with the 2804
definitions. If one looks at Figure 3 in the draft, it very clearly matches any
intuitive definition of (an API for) a wiretapping technology.  The "TLS decrypter" can be
anywhere on the Internet, so long as it has access to the key-leaking API. 
If some local government coerced some local popular web site into implementing
this scheme so that the local government has access to the content of any TLS
session with that web site, then anyone sane would consider that a wiretap on
the web site.

However, we can also easily see how the 2804 definitions are still met,
even taking a lawyerly approach...

1. With SMTP/TLS and with any intermediate MTA, (so 3 MTAs on the path) this
clearly allows that intermediate MTA to renege and leak
their private values and hence senders and recipients will
not get the confidentiality they expect. Note that SMTP/TLS
is almost ubiquituous today and that 2-TLS session setups
are common, e.g. if the intermediary MTA does AV scanning.
In this case, the relevant parties to the communication
(for the RFC2804 definition) are the mail senders and
receivers and the AV scanning MTA is the third party
that enables wiretapping, just as a telco (with whom
the caller or callee presumably has a contract) is the
third party in traditional wiretaps.

	- In case of doubts about the scale of SMTP/TLS deployment, see 
[Google's saferemail](https://www.google.com/transparencyreport/saferemail/)
statistics which shows nearly 90% of mails outbound from gmail.com being
encrypted with TLS now.  In many mail deployments there will be an added hop
e.g.  for anti-spam (we do that here in TCD/tcd.ie) to an outside party (in our
case outlook.com).  (On days when we get enough inbound mail from gmail to be
visible in their published DB;-) 100% of mails from gmail.com to tcd.ie are
sent via TLS to a host below outlook.com for AV scanning and then on to the TCD
MTA, also using TLS.  That's perhaps some 10's of thousands of mails per day 
for the 3,000 staff and 20,000 students in TCD.
I'd guess outlook have thousands of customers, so if this draft were deployed
in that AV scanning MTA infrastructure, that could put at risk the
confidentiality of tens to hundreds of millions of emails per day.  So, while
less than 100% of mail is encrypted with TLS on all hops, much is. As [pointed
out](https://www.ietf.org/mail-archive/web/tls/current/msg23843.html) on the
TLS list, the UTA working group are also developing MTA-STS to try improve that
situation, so any argument that mail senders and receivers do not expect
confidentiality is sadly outdated.

- As another example, consider (vanity domain) web sites where 
the domain holder doesn't have shell access to the 
machine that hosts the web site. (Wordpress.com has 
millions of such users, as to many many other sites.)
If the hoster in that case turns on this 
interface and provides private DH values to someone who
has access to the ciphertext TLS packets, then that is 
clearly wiretapping. That is especially clear if e.g.
to people are using such a web site to communicate
with one another via some wordpress plug-in. 
In this scenario the two people who
are using some wordpress plugin to communicate
are the first and second parties and the hoster is
the third party, in the role of the telco in a
traditional wiretap.

It does not matter that in these cases the third
party (AV scanner or hoster) has access to the
cleartext but does not supply that to the consumer
of the wiretap - whether the wiretap is based
on runtime or later decryption of captured packets,
or on playing back physical tapes of a conversation,
or on consuming a live feed from the telco-equivalent
is immaterial - the end result is that the first 
and second parties communications are being 
wiretapped according to the definitions in RFC2804.

[[I can add pictures if needed;-)]]


## Feel free to add older/other break-TLS proposal debunking text below here.

For example, about mcTLS, if you have the energy to
consider that nonsense.


