# tinfoil: TLS Is Not For Obligatory (Or Ostensibly Optional) Interception, Luckily

*20180319 update - the TLS WG discussed version -01 
of [https://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-01](https://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-01)
and there was no consensus to adopt that, so that proposal may now be dead.*

*20171009 update, I've started to document the failings of the [latest](#latest) 
proposal we're forced to deal with. Believe it or not, I did do a pass to tone down the
language already, but am happy to do more of that, if people think that'll
help the discussion. Not that this discussion can be anything but horrendous:-(*

*20171019 - just noting a few points that were raised in the initial TLS WG 
[list discussion](https://www.ietf.org/mail-archive/web/tls/current/msg24492.html)
of [draft-rehired](https://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)*

This repository is a place to collect together arguments for not breaking TLS.
By "breaking TLS" I mean significantly weakening the protocol, or
implementations or deployments of the protocol, so that the security claims
made in the protocol specification can no longer be credibly maintained.

Note that one does not need to accept all arguments in order to properly
conclude that breaking TLS is a bad plan or that some specific break-TLS
proposal is a bad plan. One killer argument per proposal should be enough, but
it's useful to collect more than that anyway.

At some point, if the TLS WG want to, it 
[may be worth turning this into an Internet-draft](https://www.ietf.org/mail-archive/web/tls/current/msg23909.html) 
to save us all time for when the next break-TLS proposal comes along.
In the meantime, reading this as if it text were in a -00 draft is
probably roughly correct if you're familiar with IETF stuff. (IOW,
this isn't perfect text and that's ok:-)

PRs that improve or add to or just record those arguments are welcome, especially if
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
weaken such an important security  protocol without due diligence is utterly
unwise.  And it is hard to see how anyone could really do that due diligence
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
interception implementations, for example [Durumeric et
al.](https://www.internetsociety.org/sites/default/files/ndss2017_04A-4_Durumeric_paper.pdf)
found that "that nearly all [interception proxies] reduce connection security
and many introduce severe vulnerabilities" whilst [de Carnavalet et
al.](http://users.encs.concordia.ca/~mmannan/publications/ssl-interception-ndss2016.pdf)
performed a "systematic analysis uncovered that several of these tools severely
affect TLS security on their host machines." So, where there is evidence
publicly available, it seems to indicate that additional complexity (via
proxies or other methods of breaking TLS) reduces security. To my knowledge,
none of the proposed "break TLS" schemes has ever offered evidence that they
would lead to an overall improvement in security.

1. In many cases like this people also argue that even if breaking TLS is
  undesirable, it's better to do that undesirable thing openly inside the IETF
as it would happen elsewhere in any case and perhaps be done worse. Without
specific evidence that organisation foo is about to take action bar, that
argument is impossible to judge, so is at best neutral. When there is specific
evidence of something relevant being done elsewhere, then the IAB and IESG have
liaison mechanisms that can be used for the purpose for which they were
designed.  IOW, "if we don't, they will" is not a good argument that the IETF
should do any particular thing at all. In the author's experience, this
argument is commonly associated with proponents of proposals that might be fairly
described as being engaged in forum shopping. In the case of "break TLS"
proposals, one could argue (and I would) that the reason TLS is widely
used is, in significant part, because TLS is not broken. 


1. <id ="iabst" name="iabst"/>In 2014, the Internet Architecture Board issued a 
[statement](https://www.iab.org/2014/11/14/iab-statement-on-internet-confidentiality/)
to the effect that "Internet depended on users having confidence that the network would protect 
their private information" and that we should "make encryption the norm for Internet traffic." 
All proposals to break or weaken TLS go against that sound advice (not repeated
here because we'd end up repeating almost all of it). 
The IAB statement 
recognises that increasingly widespread use of TLS causes issues
for network operations, but no response that amounts to attempting to break TLS
so far presented can be justified due to the huge potential cost and impact
of breaking TLS. 

1. Most or all approaches to breaking TLS seem to involve changing TLS from a
two-party protocol (ignoring CAs for now) into a multi-party protocol, but
one that mimics the behavior of TLS in order to have a chance to get deployed
on the Internet.  A multi-party transport security protocol however would
require either a substantially different API or to move up the stack to solve
whatever problem one is facing.  

1. Some people argue that 20 years (or so) ago the IETF was wrong to have
ignored the reality of NAT deployments, and draw a comparison with breaking
TLS, particularly in enterprise gateways. They draw the erroneous conclusion
that the IETF should therefore standardise ways to break TLS, as a way of
dealing with what is the sad reality for some networks,
where TLS is intercepted. Firstly, by itself that's not good logic, as it would
mean that the IETF should standardise anything that is deployed without
exercising any judgement. But most people making the argument probably don't
really mean that and implicitly think that some level of TLS interception is
unavoidable.  But regardless of what one thinks about NAT, the conclusion that
we ought therefore break TLS remains erroneous - recognising the existence of
NAT would not have directly damaged networks that do not use NAT, whereas
breaking TLS causes breakage and damages trust for all applications using TLS,
for all time - the impact here would be far broader than just the networks
already suffering TLS interception. So the equivalent argument really would be:
should the IETF have entirely given up on the end-to-end argument because of
NAT deployments or not?  And the answer to that is "no," just as the answer
when asked to standardise breaking TLS is "no." The bottom line is that the
"it's just like NAT, the network suffered because we ignored NAT, so we 
should break TLS" argument is fallacious. 

1. [Ben Kaduk](https://www.ietf.org/mail-archive/web/tls/current/msg24620.html) 
raised a general issue that is a problem whether or not break-TLS proposals
try to be an "opt-in" by 
making themselves visible to TLS clients: if they do not, then they fail
to be transparent (as in 
[draft-green](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01) 
), but if they do make themselves visible in a way that a network intermediary
can see, then "once a visible ClientHello extension is needed to enable wiretapping,
certain parties (e.g., national border firewalls) would reject/drop
*all* ClientHellos not containing that extension, thereby extorting all
clients into "opting in" to the wiretapping and effectively rendering
the "opt-in" requirement useless for those clients." And it seems hard
to propose an opt-in scheme where the opting-in or not is not 
detectable by an on-path intermediary. 

1. Even though the stated goal of these proposals is to allow monitoring
of the connection contents, exposing the session keys to a third-party
also allows said third-party to modify the connection contents, that is,
TLS loses not only confidentiality, but also integrity.

## Specific "break-TLS" proposals

<h2 id ="latest" name="latest">Current Proposals</h2>

### [draft-rhrd-tls-tls13-visibility-00](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)

Since it helps to have a pronounceable name for things, I'm personally calling
this draft [draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)
on the basis of the output of:

		$ grep "^r.h.r.d" /usr/share/dict/words
		rehired


### Arguments already raised in the [draft-green](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01) debacle:

These arguments were raised but not substantively dealt with 
in the discussion of [draft-green](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01), but remain telling. (And
again, only one killer argument needs to be accepted to 
see off this latest bad idea.) Some of the wording has
been tweaked for 
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00), 
but mainly the arguments are the same, and as good as ever.

1. The [TLS working group charter](https://tools.ietf.org/wg/tls/charters)
dated 2017-03-30 calls for improving the security of TLS, and 
this proposal involves leaking secret key material to 
unnamed third parties and hence clearly falls outside the charter.

1. TLS1.3 (and DTLS1.3) are still not finished and any
work within the IETF on this draft will put those efforts
at risk. For example, the abstract of the latest
[TLS1.3 draft](https://tools.ietf.org/html/draft-ietf-tls-tls13-21)
says that TLS is "designed to prevent eavesdropping" - changes
to that and possibly many other claims would be needed
if the TLS working group adopted this, or any similar, draft.
Those changes would not all be editorial and would likely
be time-consuming. [draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00) is worse than draft-green
in this respect as it is very unclear how the extensions
here fit into the overall lifecycle of TLS sessions between
two entities (e.g. what about 0rtt replayable data?). Adopting
this now would set back TLS1.3 by at least a year.

1. TLS1.3 has undergone significant academic and other analyses, 
(including two academic workshops,
[TRON](https://www.internetsociety.org/events/ndss-symposium-2016/tls-13-ready-or-not-tron-workshop-programme)
in 2016, and [TLS-DIV](https://www.mitls.org/tls:div/) in 2017), 
none of which
  (to my knowledge) envisaged the use of key escrow, nor of
"farms" of TLS servers using the same SSWrapDH1.
Adoption of any work in this space could require
re-doing some of the analysis work, possibly delaying TLS1.3 or (worse)
resulting in new problems being found after widespread deployment. Given that
this draft would create new active and passive attack possibilities, such new
analysis work would seem to be required, especially if proprietary 
uses of the awfully-named "tls_visibility"
extension were to emerge (which seems likely). 

	- The work done by academic analyses of TLS1.3 has been valuable in terms
of improving the quality of the IETF's output. Radical changes to TLS1.3 at this point 
will likely disincent researchers from contributing as a part of the process 
in future as they may perceive the IETF to be moving the goalposts at the
last minute, indicating that IETF participants do not value their inputs.
(I admit that is speculative, but it's based on some previous discussions on the WG 
list - it'd be good to get feedback from researchers to check.)

1. There could be similar problems caused for the QUIC protocol development
  work, as that relies upon TLS1.3 and has similar design elements that
could be perturbed if session keys are leaky.

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
ecosystem will be harmed by widespread use of the
same bad-crypto, so therefore in the case of
bad-crypto proposals such as this, it is better for
us all that there is not a single bad-crypto standard.
In [draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00) there is in fact nothing to stop
the server and third parties using crap-crypto without anyone else knowing.

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
  example, the holder of the private component of SSWrapDH1 would have to be able to handle all
updated ciphersuites before those could be used by bona-fide TLS clients and
servers.  We have seen that non-updated PKCS#1.5 crypto hardware has caused
problems with updating the crypto in TLS for decades now, and this would cause
similar problems.

### And here are some more [draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00) specific reasons to not do this:

Again, it's a pain to have to beat up on a -00, but that seems to
be what's needed, so here we go...

I'm focusing here on the fundamental errors in [draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00) and
hopefully less on the bits that could in theory be "improved." 
(Noting that "improving" is an oxymoron here;-)

1. The title and abstract are significantly misleading. This proposal enables
an active attacker on the TLS session in question. The title and abstract seem
to this reader as if designed to minimise or to attempt to obfuscate this fact.
(Previous attempts to break TLS have also apparently required such misnomers,
e.g. via the abuse of the term "passive" or "trusted proxy.")

1. There still is no real blood-brain barrier between uses of TLS that do or do
not involve data centres. The presentation and analysis here are (and must be)
woefully incomplete.  Adopting a change to such a widely deployed protocol as
TLS with such a dearth of analysis would be irresponsible of the WG and
of the IETF.

1. [draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00) claims that the third parties are  "authorized"
but that is not the case - the client has no say in which 3rd parties
end up with the keys, nor is the client even told about which
third parties get to see and possibly modify all their traffic.
And the server doesn't even have to have a name for the wiretapper.
(Adding names will not help however - this goes back to the problem
of trying to abuse a 2-party protocol for >2 parties.)

1. Despite the design-goal of client opt-in
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)
would nicely enable state-level snooping and modification of TLS traffic
*without client knowledge* - all that would be needed is to mandate that the
value of the server's TLSVisibilityExtension be provided to the surveillance
infrastucture and the TLS extensions be omitted from the handshake. Once server
code supports this as-written, then that could easily be done out of band -
there is nothing in this proposal that cryptographically *requires* the client
to even know about the leaked keys/snooping. 

1. The concept that TLS clients will "consent" to this is risible - general TLS
clients could be mandated by local regulation to emit the "please screw me"
extension.  What browser chrome would then be displayed? There are no
good answers there, nor is there any way for a TLS client user (when a person)
to choose when to send the "please screw me" extension. Due to scarce
screen real estate some applications on mobile devices will thus send this
always, utterly undermining TLS. And the so-called "consent" situation
for other applications is completely unknown.

1. Enterprise TLS clients unlucky enough to be forced to send this will not be
able to distinguish enterprise-local from remote wiretapping, and nor will
enterprise infrastructure unless it maintains an up-to-the minute white-list of
allowed wiretapper fingerprints.  In addition to not matching any sensible
enterprise policy, that would also even further encourage enterprises to MITM
all outbound TLS with all the known-bad consequences.

1. Multiparty imperfect forward secrecy: While forward secrecy is a goal
for cryptographic protocols, it is rarely perfect, despite earlier
uses of the term. In particular, performance reasons may call for use
of tickets or other structures that are stored and that could break
forward secrecy, typically if the server suffers a breach. With TLS
as-is, the trade-offs in term of how imperfect to allow one's forward
secrecy to be is essentially a matter for the TLS endpoints. With 
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00) 
both the server and the snooper are now involved in deciding how
imperfect to make forward secrecy. Given there is no real 
protocol between those (and nothing that involves the client),
the fairly predictable end-result will be very long and hard to
change durations during which sessions are at risk due to a breach
in one place. (Breach in snooper is clear, same applies in the
TLS server though, as whoever breaks in can replace the snooper's
DH with their own.) Multi-party control over cryptographic 
parameters such as this seems hugely unwise.

1. Inter-snooper relationships: If a protocol like 
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00) 
were standardised, there would be an immediate call to generalise that
to more than one snooper. In discussion of this on the ilst, 
[Florian Weimer](https://www.ietf.org/mail-archive/web/tls/current/msg24607.html)
raised the point that one might require none of the snoopers are
aware of one another, or one might require 
that all the snoopers
are aware of all the other snoopers. Similarly, the real TLS endpoints
might be required to know about some or all of the snoopers. This
situation seems laden with contradictions. For example, if the client knows
all the snoopers, then any snooper can create a new TLS session with
the same server and discover something about all the snoopers.

1. Sending the "please screw me" extension (in clear) in the ClientHello is
broadcasting/advertising the vulnerability.

1. On the other direction, the extension in the ServerHello identifies
not only the presence of the vulnerability, but also which long-term key
should be stolen to be able to decrypt the connection.

1. Yes, despite the author's efforts, this would still 
enable wiretapping as defined in 
[RFC2804](https://tools.ietf.org/html/rfc2804).
The figure below is one of possibly many scenarios to
demonstrate that: if the browser is unlucky enough to
be behind e.g., some corporate TLS MITM attacker (a not unlikely
scenario, that presumably the authors of 
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)
are happy enough to encourage) then that MITM proxy can be set
to emit the "please screw me" ClientHello extension
and any co-operating or coerced web servers will then
emit the required session key information to allow
the wiretapper to work happily on the Web. Enabling such
scenarios is an inherent consequence of trying to make TLS a >2 party
protocol.

<pre>

+-----------+  +--------------+      +----------+
|browser    +--+TLS MITM Proxy+--+---+Web server|
+-----------+  +--------------+  |   +-----+----+
                                 |         
                                 |         
                                 |        
                         -----------------
                         | TLS decrypter |
                         -----------------

                Figure: Browser wiretapping setup using 
    https://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00

</pre>

### Some not-quite editorial issues...

1. Despite the extended debates on the WG list, this paragraph of the
introduction is erroneous to the point of almost seeming disingeneous, it says:

		Specifically, the use of ephemeral ciphersuites prevents the use of
		current enterprise network monitoring tools such as Intrusion
		Detection Systems (IDS) and application monitoring systems, which
		leverage the current TLS RSA handshake to passively decrypt and
		monitor intranet TLS connections made between endpoints under the
		enterprise's control. 

	Discussion on the TLS WG list has previously discredited such overly-broad assertions,
on the basis that a) many people have asserted that these are non-problems
for their real data-centres and b) those who like static RSA key transport can
just keep using TLS1.2 *within their data-centres* for years to come. There is nothing that will prevent
anyone from continuing to do that. (That is despite FUD-like
claims about PCI-DSS - if one wanted to justify a PCI-DSS based claim
one would need to point at the specific (but non-existent) PCI-DSS 
requirement for TLS1.3 or to one that requires plaintext access outside 
the transport endpoiints.
None of the proponents of breaking-TLS has done that, despite repeated
requests.)

1. "nearly always an improvement" are pretty much weasel-words.
Either the authors agree with the IETF [BCP200](https://tools.ietf.org/html/bcp200) 
that forward secrecy is a best current practice, or they do not. If they did
agree with BCP200 they presumably would not write this
draft, which is designed to break forward-secrecy. 
If they disagree with BCP200, then they ought suggest a re-charter
for the WG or a revision of BCP200.

1. It is worth noting the irony that one of the authors of
<a href="">draft-rehired</a> was the IAB chair when the IAB correctly issued the <a href="#iabst">IAB statement</a>
referred to above. 
Given this contrast, it is fair to quote from that IAB
statement and consider how it is diametrically opposed
to 
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)
 
		The IAB urges protocol designers to design for confidential operation by
		default.  We strongly encourage developers to include encryption in their
		implementations, and to make them encrypted by default.  We similarly encourage
		network and service operators to deploy encryption where it is not yet
		deployed, and we urge firewall policy administrators to permit encrypted
		traffic.
		
		We believe that each of these changes will help restore the trust users
		must have in the Internet.  

	While one might attempt to argue that the above does not say "and oh,
by the way, it is ok to hand out session keys to some unnamed parties so 
they can decrypt ciphertext" such an
argument seems very unlikely to have been IAB member's mind whilst
writing that statement - the IAB statement's words above argue for the opposite of 
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00).

1. [draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)
says that forward secrecy "may slow adoption of TLS 1.3 or force enterprises to
continue to use outdated and potentially vulnerable technology" but then goes
on to propose adding a specific and concrete actual vulnerability (key escrow)
and one that would further ossify TLS1.3 (see above). Bad plan.  As stated
before, there's no reason why so-called "data centre" uses need to adopt TLS1.3
now, nor why such data-centre internal uses of TLS would significantly slow
adoption of TLS1.3 elsewhere.

1. TLS1.3 deliberately removed from TLS the option to send the session
secrets encrypted with a long-term key.
[draft-rehired](http://tools.ietf.org/html/draft-rhrd-tls-tls13-visibility-00)
reintroduces that possibility. That would be a step backwards in the protocol
evolution.


## Other/Older/Dead Proposals

Feel free to add analyses of older/other break-TLS here or even just links to
drafts/papers.  For now, other than
[draft-green](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01),
I've just noted a few that'd deserve coverage if [the TLS WG think documenting
those would be
useful](https://www.ietf.org/mail-archive/web/tls/current/msg23909.html).

### [draft-green-tls-static-dh-in-tls13-01](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01)

This July 2017 draft was a proposal for breaking TLS that
was discussed at IETF 99, didn't [come anywhere near achieving
rough consensusa](https://www.ietf.org/mail-archive/web/tls/current/msg24172.html) and so is hopefully dead.

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
in 2016, and [TLS-DIV](https://www.mitls.org/tls:div/) in 2017), 
none of which
  (to my knowledge) envisaged the use of static DH values, nor of centrally
generated DH values being "pushed" to many TLS servers. Similarly, work on the
TLS application layer PDU protection didn't envise re-use of DH values in
possibly many TLS sessions.  Adoption of any work in this space could require
re-doing some of the analysis work, possibly delaying TLS1.3 or (worse)
resulting in new problems being found after widespread deployment. Given that
this draft would create new active and passive attack possibilities, such new
analysis work would seem to be required, especially if proprietary 
extensions to the proposed "TSK" protocol affect the cryptography used
in TLS sessions. 

	- The work done by academic analyses of TLS1.3 has been valuable in terms
of improving the quality of the IETF's output. Radical changes to TLS1.3 at this point 
will likely disincent researchers from contributing as a part of the process 
in future as they may perceive the IETF to be moving the goalposts at the
last minute, indicating that IETF participants do not value their inputs.
(I admit that is speculative, but it's based on some previous discussions on the WG 
list - it'd be good to get feedback from researchers to check.)

1. Kenny Paterson (private communication, quoted with permission) notes that
history has shown that static DH in TLS (and elsewhere) is not "implementation
robust" - another relevant attack is
[this](http://nds.rub.de/media/nds/veroeffentlichungen/2015/09/14/main-full.pdf)
one which is quite spectacular.

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
Figure 3 of the draft (reproduced below) clearly describes a generic 
key-leaking/wiretapping architecture for TLS that could (and would)
be used in many other circumstances that the authors
have apparently not envisaged.

	- As pointed out on the list (e.g. by 
[Yoav Nir](https://www.ietf.org/mail-archive/web/tls/current/msg24070.html) and others), if there was
a standard for this scheme, code for that would leak into being
used on the public Internet via the normal implementations that
are used both within and outside pretty much all networks.

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
documented in RFC so far have been in the independent
stream (hence not IETF documents) or perhaps pre-date
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
a function (passing in initial packets) to get a key for 
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

1. Aside from the process/BCP perspective on "bad crypto" Nick Sullivan 
[pointed out](https://www.ietf.org/mail-archive/web/tls/current/msg23962.html) 
issues with the specific proposal: "Static Diffie-Hellman is a
cryptographically problematic construction. Not only was it found to be fragile
to implement in the prime field variant ([LogJam](https://weakdh.org/)), the
Elliptic Curve variant has recently been identified as troublesome as well (see
recent [JWE
vulnerability](https://blogs.adobe.com/security/2017/03/critical-vulnerability-uncovered-in-json-encryption.html)
and
[CVE-2017-8932](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-8932)).
Furthermore, many post-quantum key exchange mechanisms cannot be secured with
repeated key shares (SIDH is one example).  Encouraging (or worse,
standardizing) the repeated use of a key share seems risky and shortsighted.  "

1. The argument has been made that it would be better
to scrutinise proposals such as this openly in the IETF
instead of having individual vendors develop their
own bad-crypto implementations. As a counter to that,
while scrutiny is good for good crypto, the overall
ecosystem will be harmed by widespread use of the
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
into ongoing sessions. The abstract of the -01 draft is extremely misleading in
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
the servers.  At a quick glance it seems to me that [draft-green](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01) doesn't
separate this issue from the rest. But server farms have no choice about
solving that problem, or the intrusion/DOS detection problem, but they do have
a choice about monitoring as such.  Separating the operational requirements
from the political requirements might be a good first step."

1. IPR: As of 20170727, there are a couple of [IPR declarations](https://datatracker.ietf.org/ipr/search/?id=draft-green-tls-static-dh-in-tls13&submit=draft) to be considered.

1. Paul Wouters mentioned that the IETF is deprecating some DH groups

1. mnot: https is 2 party, there's no way to get informed consent to change
that, same here

1. Ted Hardie: PFS is a feature, you can't tell on 1st connection for sure that it's
been removed and that's a basic feature, and hence needs to be indicated,
this doesn't meet that basic protocol feature.

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

1. [Section 7.2](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01#section-7.2)
defines a key request interface, but defines no authorisation
scheme, no HTTP authentication, and requires no additional CMS
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

1. Richard Barnes [points out](https://www.ietf.org/mail-archive/web/tls/current/msg23793.html)
that there may be "unstated requirements here to support for resumption
(without having to find the previous session) and probably 0xRTT, and those
modes use in a shared secret in addition to the DH secret.  So you would need
to exfiltrate the PSK as well." If correct, that would seem to 
indicate that this proposal is broken. 

1. The use of CMS means that there are already a vast array of possible (even
  deployment-specific) extension points in this protocol. That means that it is
not possible to have any confidence that what would be designed is what would
be deployed. In fact, given the detectability issues above, it seems very
likely that non-detection (meaning non-compliance with the putative RFC) will be
a goal for implementers.

IMO none of the above ought not be fixed by IETF participants collectively.

#### Why is this wiretapping?

Some IETF TLS WG list participants have denied that this is a wiretapping
proposal, based on it somehow not matching the definitions in 
[RFC2804](https://tools.ietf.org/html/rfc2804).
Firstly, that is far too lawyerly and IMO being overly pedantic with the 2804
definitions. If one looks at Figure 3 in the draft, it very clearly matches any
intuitive definition of (an API for) a wiretapping technology.  The "TLS decrypter" can be
anywhere on the Internet, so long as it has access to the key-leaking API. 
If some local government coerced some local popular web site into implementing
this scheme so that the local government has access to the content of any TLS
session with that web site, then I think it'd be insane to not consider that a wiretap on
the web site in question.

<pre>
                    --------------       -----------------
                    | TLS server |-------|  key manager  |
                    --------------       -----------------
                           |                     |
                           |                     |
                           |                     |
                           |             -----------------
                           |------------>| TLS decrypter |
                           |             -----------------
                           |
                           |
                    --------------
                    | TLS client |
                    --------------


                     Figure 3: TSK protocol components
(copied from https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01)
</pre>

However, we can also easily see how the 2804 definitions are still met,
even taking a lawyerly approach...

1. With SMTP/TLS and with any intermediate MTA, (so 3 MTAs on the path) this
clearly allows that intermediate MTA to renege and leak
their private values and hence senders and recipients will
not get the confidentiality they expect. Note that SMTP/TLS
is almost ubiquitous today and that 2-TLS session setups
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

<pre>

+-----------+  +--------------+      +----------+     +---------+  +-------------+
|mail sender+--+Submit server +--+---+AV scanner+-----+Recip MTA+--+mail receiver|
+-----------+  +--------------+  |   +-----+----+     +---------+  +-------------+
                                 |         |
                                 |         |
                                 |         |
                  -----------------       ----------------
                  | TLS decrypter |-------| key manager  |
                  -----------------       ----------------

                Figure: SMTP/TLS wiretapping setup using 
    https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01

</pre>

1. As another example, consider (esp. vanity domain) web sites where 
the domain holder doesn't have shell access to the 
machine that hosts the web site. 
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

	- And again, this is a real scenario: [wordpress.com](https://wordpress.com/) 
has millions of such users, as do many many other hosted 
sites where those communicating do not have access to 
a shell or to the TLS server configuration.

<pre>

+--------------+       +---------------+        +--------------+
|blog commenter+---+---+Hosted Web Site|----+---+blog commenter|
+--------------+   |   +-------+-------+    |   +--------------+
                   |           |            |
                   |           |            |
                   |    -------+--------    |
                   |    | key manager  |    |
                   |    ----------------    |
                   |           |            |
                   |           |            |
                   |   --------+--------    |
                   +---+ TLS decrypter +----+
                       -----------------

                Figure: Hosted web site wiretapping setup using 
    https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01

</pre>

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

### [TLS-RaR](https://nsr.cse.buffalo.edu/mobisys_2017/papers/pdfs/mobisys17-paper32.pdf)

From the abstract: "This paper presents TLS–Rotate and Release (TLS-RaR),
a system that allows device owners (e.g., consumers, security
researchers, and consumer watchdogs) to authorize devices,
called  auditors,  to  decrypt  and  verify  recent  TLS  traffic
without compromising future traffic."

Analysis TBD.


### [mcTLS](https://mctls.org/)

This was a [sigcomm publication](http://www.cs.cmu.edu/~dnaylor/mcTLS.pdf)
(sigcomm went down in my estimation for accepting that) assuming that it makes
sense for the random IP address (of a middlebox) to be somehow authorised by
the endpoints in a TLS sesssion to join in as a 3rd party in a TLS session.

### http/2 Related Proposals

These drafts were proposed during the development of http/2.

- In 2014: [draft-loreto-httpbis-trusted-proxy20-01](https://tools.ietf.org/html/draft-loreto-httpbis-trusted-proxy20-01)
- In 2013: [draft-vidya-httpbis-explicit-proxy-ps-00](https://tools.ietf.org/html/draft-vidya-httpbis-explicit-proxy-ps-00)
- In 2012: [draft-rpeon-httpbis-exproxy-00](https://tools.ietf.org/html/draft-rpeon-httpbis-exproxy-00)

### [draft-mcgrew-tls-proxy-server-01](https://tools.ietf.org/html/draft-mcgrew-tls-proxy-server-01)

This was a draft from people working on so-called "Next Generation Firewalls". 
Such firewall products typically include a client-side TLS proxy that has a 
Certification Authority (CA) and signs "fake certificates" (that is what the 
vendors call them internally) for the websites that clients behind those proxies
attempt to connect to. 

The interception is usually visible to the clients, because they have to be configured 
to trust the interception CA, but in some [well-publicized cases](https://nakedsecurity.sophos.com/2013/01/08/the-turktrust-ssl-certificate-fiasco-what-happened-and-what-happens-next/)
(mis-)trusted CAs issued sub-CA certificates for such purposes. Even when used as intended,
the interception is invisible to the TLS server. The interception also hinders the client's 
ability to validate the server certificate, because the client only sees the fake certificate.
This prevents the client from displaying the Extended Validation indication for server certificates.

The draft was intended to make the real certificate visible to the client and to make the 
interception detectable to the server in order to alleviate those limitations. The TLS WG 
did not want to adopt this work, and the draft was abandoned.




