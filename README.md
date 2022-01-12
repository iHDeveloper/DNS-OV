---
CIP: XX
Title: DNS-OV Standard
Authors: Hamza Aljuaid (iHDeveloper) <mail@ihdeveloper.me>
Comments-URI: https://forum.cardano.org/t/fair-min-fees-cip/47534
Status: W.IP
Type: Standards
Created: 2022-01-13
License: CC-BY-4.0
Requires: CIP-0010 Transaction Metadata Label Registry 
---

# CIP XX - DNS-OV (DNS Ownership Verification) Metadata Standard [W.I.P]
A way to verify the ownership of a certain DNS and validate if non-authorized change
has happened to the reocrds.

## Abstract
Using the current DNS infrastructure on the blockchain is currently facing a
problem. It's not possible to verify the ownership of the domain over time.
The current infrastructure's way is by having a central point checking it for you.

We will propose a new metadata standard for providing this unique ability that will put
Cardano blockchain as a "decentralized verifier" for the current DNS infrastructure.

This will help Dapps on Cardano blockchain to use the current DNS infrastructure that's
adopted by billions of devices and users while keeping a trustless proof of their ownership.

## Motivation

In the blockchain industry, There's has been discussions about running DNS on a blockchain.
Some discussions raised the question of "decentralization?" and decided to rewrite the DNS protocol.
And, some discussions agreed to run a DNS rootnode that uses the blockchain as a market for owning "domains" and trading their ownership as an asset.

However, the reventing of the DNS protocol is facing a challenge of "adoption" and
the rebuilding of an infrastructure against the current infrastructure that has been built
over the users and contributed by many.

And, the idea of running a rootnode for a certain blockchain raises the question of "decentralization".
Because, running a DNS rootnode requires ownership. So, the blockchain is used to decide who owns which domain.
But, in the end there are some owners controlling this rootnode and without it. These domains will mean nothing.

In this proposal, we will present a new  data structure that will use Cardano blockchain
as a "decentralized verifier" to proof the ownership of the domain owners in the current DNS infrastructure
even in the face of adverseries (un-authorized manipulation of records, change of ownership, etc...)
 
## Specification
This section is under **W.I.P**.

### Hash Reference
When referencing a DNS there should be a hash reference to detect
any manipulation of the certificate transaction ID reference.

The hash is composed of the domain name + the transaction id of the certificate.

### TXT Records

The record format will be `dns-ov-key=value`.

| Key | Value |
|-----|-------|
| txid | The transaction Id of the certificate |
| msg | The contents of the signed message |
| sigmsg | The signed message by the private key |

### Message Content
This section is W.I.P due to me recently just knew that there's a limit on the TXT record size (255 bytes).

but the possibilities here are interesting such as
- including the sign date and the expire time (like in `3h`, `6h`, 1d`, etc...)
- including a hash of records that **shouldn't** be changed until the next updated signed message.

### Certificate Metadata

```json
{
    "XX": {
        "name": "THE DOMAIN OF THE CERTIFICATE",
        "public-key": "THE PUBLIC KEY OF THE CERTIFICATE",
        // recovery proof in case the public key has been lost (WIP)
    }
}
```

### Verifier Node
A simple node that will receive the domain name and the hash reference.
It fetches the TXT records using a domain lookup.

Then, it checks the domain name and the transaction id combined hash is the same as the hash reference provided by the user.

Then, it will fetch the transaction body from the blockchain.
It validates the signed message with the public key that's stored in the certificate
thats inside the transaction metadata.

Then, it checks the message content if the expire date since the signed date is past the current time or not.

If all checks out, then it notifies the user that this domain name has proven its ownership
Otherwise, it notifies them something went completely wrong.



## Copyright

This CIP is licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/legalcode)


