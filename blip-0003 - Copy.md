```
bLIP: xxxx
Title: NameDesc
Status: Draft
Author: Hampus Sjöberg <hampus.sjoberg@protonmail.com>
Created: 2022-01-14
License: CC0
```

## Abstract

NameDesc is a standard for conveying "receiver name" in BOLT11 invoices.  

This bLIP serves to document what is already supported by some wallets (see
[Reference Implementations](#reference-implementations)), for posterity and so
that new implementations don't have to reverse-engineer general-purpose
NameDesc from existing implementations. 

## Copyright

This bLIP is licensed under the CC0 license.

## Motivation

From a UX point of view, the recipient field allow for improvements in the transaction logs, like displaying 

## Specification

NameDesc consists in prepending the actual invoice description with a name plus two spaces. For example, Alfred's wallet can produce an invoice with the following description:

```
Alfred: payment for the ice cream you owed me
```

When Barbara's wallet receives that, it displays something like:

```
Paying to: Alfred
Description: payment for the ice cream you owed me
```

## Rationale

The good thing about such a simple standard is that if a wallet doesn't support parsing the "Name" part it will still not be bad, as it will show:

```
Description: Alfred:  payment for the ice cream you owed me
```

And if the payee's wallet doesn't support including a "Name", still Alfred can manually type his name in the description field. Not the best experience in the world, but doable.

## Universality

This proposal lives in the application layer. It is not necessary for a lightning implementation to support/insert what the blip does here.

## Backwards Compatibility

This does not have backwards compatibility concerns

## Reference Implementations

* Blixt: <https://github.com/hsjoberg/blixt-wallet/commit/e0ab93fdbaa92ec56bf920803bae22bf9108dfc4>
* lntxbot: <https://github.com/fiatjaf/lntxbot/commit/3bde760066ad5ae20e0672a53cdbd910f14d7149>