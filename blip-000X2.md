```
bLIP: xxxx
Title: Podcast 2.0 Boosts and Boostagrams
Status: Active
Author:  Satoshis Stream <satoshisstream@pm.me>
Created: 2022-01-24
License: CC0
```

## Abstract

Using Podcast 2.0 apps, listeners can send sats (boost) to the hosts of the podcasts they listen. This is part of what is known as "the Value for Value system". These apps (see
[Reference Implementations](#reference-implementations)) adopted the 7629169 TLV type for inputting key-value JSON metadata about the sent payment. The TLV holds data about the timestamp when the payment was sent within the episode, the name of the podcast and its [Guid](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#guid), among other fields described in the [Specification](#Speciciation) section. 

Example: `{'podcast': 'PODCASTNAME', 'feedID': 1337, 'episode': 'EPISODENAME', 'action': 'boost', 'ts': 33 }`

## Copyright

This bLIP is licensed under the CC0 license.

## Specification

The sender of the payments (the Podcast app) needs to input some metadata so the podcast host can identify information around the timestamp the payment was sent and messages his listener may have attached to it (often called "boostagrams").

_It is allowed to send a batch of payments, by sending a **list** of above key-value "blocks" . If you do, consider setting value_msat per block._

Identifying the podcast **required**: use any of `podcast`, `feedID` or `url`. **guid preferred**
* `podcast` (str) Title of the podcast
* `feedID` (int) ID of podcast in PodcastIndex.org
* `url` (str) RSS feed URL of podcast
* `guid` (str) [The `<podcast:guid>` tag](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md#guid).

Identifying the episode **recommended**: use any of `episode`, `itemID` or `episode_guid`. **itemID preferred**
* `episode` (str) Episode of the podcast
* `itemID` (int) ID of episode in PodcastIndex.org
* `episode_guid` (str) The GUID of the episode

Information about time **recommended**: use any of `time`, `ts`. **ts preferred**
* `time` (str) HH:MM:SS timestamp when the boost/stream was sent WITHIN the episode (playback position)
* `ts` (int) Timestamp when the boost/stream was sent WITHIN the episode (playback position)

Rest of keys:
* `action` **recommended**: (str) "boost" or "stream"
* `app_name`: **recommended** (str) Name of sending app
* `app_version`: (str) Version of sending app
* `boost_link`: (str) App specific URL containing route to podcast, episode, and timestamp at time of boost.
* `message` (str) Text message to add to (boost) message
* `name` **recommended** (str) Name for this split in value tag
* `pubkey` (str) Sending node pubkey
* `sender_key` (str) Node key of sending
* `sender_name` (str) Name of sender (free text, not checked by signatures)
* `sender_id` (str) Static random identifier for users, not displayed by apps, for abuse purposes. Apps can set this per-feed or app-wide. A GUID-like random identifier or a hash works well. Max 32 ascii characters.
* `sig_fields` (str) pipe separated list of fields that are used for signature (example: feedID|itemID|ts|action|sender_key|message)
* `signature` (str) DER-encoded ECDSA signature
* `speed` (str) Speed
* `uuid` (str) Unique UUID
* `value_msat`: (int) Number of millisats for this split payment
* `value_msat_total`: (int) TOTAL Number of millisats for the payment (all splits together, before fees. The actual number someone entered in their player, for numerology purposes.)

## Motivation

The Value for Value system is a way for podcasters to receive direct payments from listeners in the form of bitcoin through the Lightning Network. It provides not only a new way for podcast to earn and keep their independency, but also new forms of interaction between podcasters and its listeners.

Using the same TLV type allow for podcast creators to understand the messages regardless of which podcast app it was sent from and which solution they used to receive their payments.

## Rationale

The TLV type 7629169 was originally chosen by Breez and other applications adopted it to keep the same standard. 

The required and recommended fields are constantly evolving by iterations from different Podcast 2.0 apps. Some of these fields are related to the extensions proposed by [PodcastIndex.org](https://podcastindex.org/) to the RSS 2.0 spec in order to deliver new functionality to apps and aggregators (see [the Podcast Namespace](https://github.com/Podcastindex-org/podcast-namespace))

## Universality

This usage of the 7629169 TLV type is only intended for Lightning applications in the Podcasting 2.0 space. Lightning applications that do not intersect with Podcasting 2.0 have no need to implement this standard. Additionally, no additional work is required from a Lightning implementation itself beyond BOLT-specified TLV support in order to enable this use case at the application layer.

## Backwards Compatibility

This is not using a feature bit and the TLV record is oddly typed, so there are no concerns regarding backwards compatibility. 

## Reference Implementations

* Breez: https://github.com/breez/breezmobile/blob/4cf057c066d03c155964f0c4db43476cd70a57ab/lib/bloc/podcast_payments/podcast_payments_bloc.dart
* Podverse: https://github.com/podverse/podverse-shared/blob/fff84c0143dea4fa01241109b8666d4c0b9a6ffc/src/satoshiStream.ts
* PodStation: https://github.com/podStation/podStation/pull/249
* Helipad: https://github.com/Podcastindex-org/helipad/blob/203e72dafb65e4f9e89540fbe4fc07a456a9907a/src/main.rs