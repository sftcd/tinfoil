# tinfoil: TLS Is Not For Obligatory Interception Lovers

This repository is a place to collect together arguments
for not breaking TLS. Note that one does not need to
accept all arguments in order to properly conclude
that breaking TLS is a bad plan or that 
some specific break-TLS proposal is a bad plan. One
killer argument per proposal should be enough, but
it's useful to collect more than that anyway.

At some point it may be worth turning this into an
Internet-draft to save us all time for when the next
crappy break-TLS proposal comes along.

PRs that add to (really, record) those arguments are
welcome, especially if they identify problems with specific 
proposals to break TLS.

## Meta-Points

In this section we call out some generic issues that all
cause all "break-TLS" proposals to fail.

- With all break-TLS attempts so far, the proponents
have only analysed the deployments about which
they care, and ignore all other uses of TLS. There are
many many uses of TLS, for example the TLS1.2 RFC is
currently (20170711) [referenced by](https://datatracker.ietf.org/doc/rfc5246/referencedby/) 434 other IETF specifications, and 
is [cited](https://scholar.google.com/scholar?q=http%3A%2F%2Fwww.hjp.at%2Fdoc%2Frfc%2Frfc5246.html&btnG=&hl=en&as_sdt=0%2C5) by 3,281 publications
in Google Scholar. Screwing around with such a protocol without
due dilligence is utterly unwise.

- Note that I make no allegations about the bona-fides
of any of the proponents of the "break-TLS" schemes.
I know and respect some of them, but they are misguided.
However, we cannot ignore the fact that some governments
are very keen to weaken Internet security and privacy 
and have allocated significant budgets for that.
[BULLRUN](https://www.theguardian.com/world/2013/sep/05/nsa-gchq-encryption-codes-security)
for example is reported to have involved wasting/spending US$250M/yr
on this. It is inevitable that some of that money ends up
being spent/wasted on schemes to break or weaken TLS.
The consequence of that is that it is entirely proper
to consider the [Pervasive Monitoring](https://tools.ietf.org/html/rfc7258) aspects of any 
"break-TLS" proposal as part of our [threat model](https://tools.ietf.org/html/rfc7624)
no matter what motivations are
set out by proponents. (And again, that says nothing
at all about proponents motivations, it's just a thing
that we have to consider regardless.)

- In 2014, the Internet Architecture Board issued a 
[statement](https://www.iab.org/2014/11/14/iab-statement-on-internet-confidentiality/)
to the effect that "Internet depended on users having confidence that the network would protect 
their private information" and that we should "make encryption the norm for Internet traffic." 
All proposals to break or weaken TLS go against that sound advice.

- TLS is already hard, as we have seen from two decades of exploits against
  implementations and, occasionally, against the protocol itself. The added
complexity of any "break TLS" proposal makes that situation worse. There is
evidence for this from peer-reviewed academic studies and examination of TLS
interception implmentations, for example [Durumeric et
al](https://www.internetsociety.org/sites/default/files/ndss2017_04A-4_Durumeric_paper.pdf)
found that "that nearly all [interception proxies] reduce connection security
and many introduce severe vulnerabilities" whilst [de Carnavalet et
al](http://users.encs.concordia.ca/~mmannan/publications/ssl-interception-ndss2016.pdf)
performed a "systematic analysis uncovered that several of these tools severely
affect TLS security on their host machines."
So, where there is evidence publicly available, it seems to indicate
that additional complexity (via proxies or other methods of
breaking TLS) reduces security. To my knowledge, none of the
proposed "break TLS" schemes has ever offered evidence
as to their effectiveness.

- Most or all approaches to breaking TLS seem to involve changing TLS from a
two-party protocol (ignoring CAs for now) into a multi-party protocol, but
one that mimics the behavior of TLS in order to have a chance to get deployed
on the Internet.  A multi-party transport security protocol however would
require either a substantially different API or to move up the stack to solve
whatever problem one is facing.  


## Specific "break-TLS" proposals

### [draft-green-tls-static-dh-in-tls13-01](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01)

This is the current (July 2017) proposal for breaking TLS.
It fails in the following ways:

- The [TLS working group charter](https://tools.ietf.org/wg/tls/charters)
dated 2017-03-30 calls for improving the security of TLS, and 
this proposal involves leaking private key material and hence
clearly falls outside the charter.

- TLS1.3 (and DTLS1.3) are still not finished and any
work within the IETF on this draft will put those efforts
at risk. For example, the abstract of the latest
[TLS1.3 draft](https://tools.ietf.org/html/draft-ietf-tls-tls13-21)
says that TLS is "designed to prevent eavesdropping" - changes
to that and possibly many other claims would be needed
if the TLS working group adopted this, or any similar, draft.
Those changes would not all be editorial and would likely
be time-consuming.

- As a result of the discussion of this draft, Christian Huitema has proposed a
[pull request](https://github.com/tlswg/tls13-spec/pull/1049) for TLS1.3 to
encourage implementers to effectively make the idea in this draft harder to deploy,
by documenting the core static DH value idea as an attack against which
TLS clients ought attempt to defend themselves.
Adopting work on static DH values would therefore have the effect of forcing
the working group to work against itself, further delaying and putting TLS1.3
at risk.

- TLS1.3 has undergone significant academic and other analyses, none of which
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

- There could be similar problems caused for the QUIC protocol development
  work, as that heavily relies upon TLS1.3 and has similar design elements that
could be pertubed if static DH private values are used. And QUIC has recently
been
[proposed](https://www.ietf.org/mail-archive/web/quic/current/msg01878.html) as
a substrate for 5G efforts in another SDO with fairly aggressive timelines, so
delays in developing QUIC caused by breaking TLS could have significant effects
elsewhere.

- This draft aims/claims to enable key leaking/wiretapping only "within"
enterprise networks, but there is no way (and cannot be a way)
to constrain the use of this scheme to such networks.
Figure 3 of the draft clearly describes a generic 
wiretapping architecture for TLS that could (and would)
be used in many other circumstances that the authors
have apparently not envisaged.

- We also have a documented case where a law enforcement agency 
has attempted to coerce a mail service provider 
called [Lavabit](https://en.wikipedia.org/wiki/Lavabit) into
providing TLS secret key materials. Developing an API
such as envisaged in this draft would encourage such
law enforcement and other agency attempts at key recovery in many countries.
So the unintended uses for this are as real as the intended 
ones. (An aspect that is ignored by the draft and it's
proponents.)

- This draft is targeted as being an IETF standards track document
but is a wiretapping scheme (or enables wiretapping) that meets the definition in
[RFC2804](https://tools.ietf.org/html/rfc2804) and therefore
cannot be a standards-track work item in the IETF.

	- Some may argue that even if this cannot be an
IETF standards track specification, it should still be
further developed by the IETF. IMO, that also goes against RFC2804,
which calls for documentation, and not development,
of deployed wiretapping schemes. Documenting the
wiretapping schemes that exist is a good thing. 
Developing new ones is a bad thing, for all the
reasons set out explicitly in RFC2804. As a 
speculative new wiretapping design, this draft
should not be an IETF specification of any kind, 
so long as RFC2804 has not been obsoleted, and
yet the authors here are making no attempt to
obsolete 2804, and are thus attempting an end-run
around IETF processes.

	- This proposal entirely suits what governments doing
pervasive monitoring would need in an API. The IETF
has documented its consensus that [Pervasive Monitoring is an Attack](https://tools.ietf.org/html/rfc7258)
in RFC 7258, and this API would assist with such 
attacks. In case this seems somewhat abstract, we
have evidence of exactly this kind of key exfiltration
API in IPsec, as documented by [Wouters](https://nohats.ca/wordpress/blog/2014/12/29/dont-stop-using-ipsec-just-yet/)
and [Der Spiegel](http://www.spiegel.de/media/media-35515.pdf).

- This draft tries to standardise broken crypto - forward
secrecy is a goal of cryptographic protocols and this 
draft deliberately aims to not provide forward secrecy
and indeed to break forward secrecy without the TLS
client being aware of that. [BCP200](https://datatracker.ietf.org/doc/rfc1984/)
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

- Ironically, the first author of [draft-green](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01)
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

- The argument has been made that it would be better
to scrutinise proposals such as this openly in the IETF
instead of having individual vendors develop their
own bad-crypto implementations. As a counter to that,
while scrutiny is good for good crypto, the overall
ecosystem will be harmed by wide-spread use of the
same bad-crypto, so therefore in the case of
bad-crypto proposals such as this, it is better for
us all that there is not a single standard.

- A [suggestion](https://www.ietf.org/mail-archive/web/tls/current/msg23802.html)
was made on the TLS list that the deployment
of this scheme be made part of a "website's 
terms of service." Hiding the fact that one is
breaking TLS in legalese seems like a terrible
idea to this user of the web.

- This draft doesn't allow normal TLS clients and applications
above or behind the TLS server to detect that wiretapping
is occurring.

- This proposal will either end up allowing TLS clients
(perhaps via zmap-style surveys) to detect that the
broken crypography is being used or it will be 
invisible to clients. If the scheme is detectable, then
it will likely fall out of use and end up worthless as the
reputational cost would be high unless this is very
widely deployed, in which case the impact of exploits
of the inevitable vulnerabilities
would be gigantic.

- Any IETF effort towards
"improving" or "scrutinising" this scheme would therefore
be wasted with high probability. In fact, developing
this interface within the IETF would arguably increase 
the probability of "worse solutions" being deployed
as those would be easier to integrate. An argument
for this draft was [offered](https://www.ietf.org/mail-archive/web/tls/current/msg23817.html)
that standardising it would avoid such "worse
solutions" - I see no evidence offered and claim
(with the same evidence;-) that the opposite would eventuate.

- More likely, if the scheme is initially detectable,
then deployments will make modifications to the scheme
to attempt to avoid detection. Such proprietary 
modifications would mean that all effort to "improve"
or "scrutinise" this scheme would certainly be 
wasted. Proprietary modifications could also significantly
weaken security.

- This scheme enables active attacks - once the DH
private values are known, then all session keys are
known and seemingly valid packets can be injected
into ongoing sessions. The abstract of this draft
is extermely misleading in claiming that it is
for "passive monitoring." No mention is made in
the draft of the active attacks it enables.

- If this scheme were widespread, it would be 
"accidentally" deployed in places for which it
was not intended. (See also RFC2804.) The result
could be active attacks on e.g. widely downloaded
web resources enabling active attacks on browsers.
I would expect another decade of academic 
publications and real exploits on TLS to result.

- Applications built on TLS, e.g. the Web, would
need to revise their security models to take
into account this scheme, as it changes the
threat model on which they have been built.

- For the enterprise uses claimed to justify this,
there is no need for Internet-scale interoperability
as the enterprise network is by-definition under
a single entity's control. (And if they cannot
control their network, then adding broken crypto
seems even more unwise.)

- Ossification: this draft would introduce new ways of
ossifying TLS, for example, the so-called "TLS decrypter" would
have to be able to handle all updated ciphersuites before
those could be used by bona-fide TLS clients and servers.
We have seen that non-updated PKCS#1.5 crypto hardware
has caused problems with updating the crypto in TLS
for decades now, and this would cause similar problems.

- Brian Carpenter points out (private communication, quoted
with permission) that
"server load balancing and TLS load balancing don't
mix well. (See [RFC7098](https://tools.ietf.org/html/rfc7098) for more.) In fact, there's no practical alternative
for large server farms except TLS proxying in front of the servers.
At a quick glance it seems to me that draft-green doesn't separate
this issue from the rest. But server farms have no choice about solving
that problem, or the intrusion/DOS detection problem, but they do have
a choice about monitoring as such.
Separating the operational requirements from the political requirements
might be a good first step."

While I'm reluctant to comment on the details of a bad
design, there are a couple of things to point out: 

- The fact that this draft both breaks and depends
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
created by these schemes called out in BCP200 and RFC2804.
There is ample recent evidence [Wannacry](https://en.wikipedia.org/wiki/WannaCry_ransomware_attack)
that many enterprises are not capable of shutting down
or controlling such interfaces that either ought not be
offered or need to be protected.

- Richard Barnes [points out](https://www.ietf.org/mail-archive/web/tls/current/msg23793.html)
that there may be "unstated requirements here to support for resumption
(without having to find the previous session) and probably 0xRTT, and those
modes use in a shared secret in addition to the DH secret.  So you would need
to exfiltrate the PSK as well." If correct, that would seem to 
indicate that this proposal is broken. (But IMO ought not be fixed
by the IETF.)

#### Why is this wiretapping?

Some of the authors have denied that this is a wiretapping
propospal, based on not matching the definitions in RFC2804.
However, with SMTP/TLS and with any intermediate MTA, this
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

In case of doubts about the scale of SMTP/TLS deployment, see [Google's
saferemail](https://www.google.com/transparencyreport/saferemail/) statistics
which shows nearly 90% of mails being encrypted with TLS now.  In many mail
deployments there will be an added hop e.g.  for anti-spam (we do that here in
tcd) to an outside party. Exactly that happens in my university where according
to the Google data above (at least on days when we get enough inbound mail from
gmail;-) 100% of mails from gmail.com to tcd.ie are sent via TLS to a host
below outlool.com for AV scanning and then on to the TCD MTA, also using TLS.
That's perhaps some 10's of thousands of mails per day. I'd guess outlook have
thousands of customers, so if this draft were deployed in that AV scanning MTA
infrastructure, that could put at risk the confidentiality of tens to hundreds
of millions of emails per day.
So, while less than 100% of mail is encrypted with TLS on all
hops, much is. As [pointed out](https://www.ietf.org/mail-archive/web/tls/current/msg23843.html) 
on the TLS list, the UTA working group are also developing MTA-STS
to try improve that situation, so any argument that mail senders
and receivers do not expect confidentiality is sadly outdated.

If one of those external parties implements your
scheme then mail senders and receivers will not know and
that real TLS application meets the 2804 definition for
lots and lots and lots of emails.


As another example, consider (vanity domain) web sites where 
the domain holder doesn't have shell access to the 
machine that hosts the web site. (Wordpress.com has 
millions of such users, as to many many other sites.)
If the hoster in that case turns on this 
interface and provide private keys to someone who
has access to the ciphertext TLS packets, then that is 
wiretapping. In this scenario two people who
are e.g. using wordpress comments to communicate
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


## Feel free to add older/other break-TLS proposal debunking text below here.

For example, about mcTLS, if you have the energy to
consider that nonsense.


