# tinfoil
TLS Is Not For Obligatory Interception Lovers

This reposotiry is a place to collect together arguments
for not breaking TLS.

At some point it may be worth turning this into an
Internet-draft to save us all time for when the next
crappy break-TLS proposal comes along.

PRs that add to (really, record) those arguments are
welcome, especially if they identify problems with specific 
proposals to break TLS.

## [draft-green-tls-static-dh-in-tls13-01](https://tools.ietf.org/html/draft-green-tls-static-dh-in-tls13-01)

This is one of the current proposals for breaking TLS.
It fails in the following ways:

- The [TLS working group charter](https://tools.ietf.org/wg/tls/charters)
dated 2017-03-30 calls for improving the security of TLS, and 
this proposal involves leaking private key material and hence
falls outside the charter.

- This draft is targeted as being an IETF standards track document
but is a wiretapping scheme that meets the definition in
[RFC2804](https://tools.ietf.org/html/rfc2804) and therefore
cannot be a standards-track work item in the IETF.

###Why is this wiretapping?

Some of the authors have denied that this is a wiretapping
propospal, based on not matching the definitions in RFC2804.
However, with SMTP/TLS and with any intermediate MTA, this
clearly allows that intermediate MTA to renege and leak
their private values and hence senders and recipients will
not get the confidentiality they expect. Note that SMTP/TLS
is almost ubiquituous today and that 2-TLS session setups
are common, e.g. if the intermediary MTA does AV scanning.



##Feel free to add older/other break-TLS proposal debunking text below here.

For example, about mcTLS, if you have the energy to
consider that nonsense.


