---
id: learn-certificate
title: Block Certificate
---

# Block Certificate

## What Is Certificate?

Block certificate holds the proof of commitment for the block.

## Certificate Structure

Each block certificate contains following information:

:::: tabs type:border-card

::: tab 🦀 Rust

```rust
#[derive(Encode, Decode)]
#[cbor(map)]
pub struct Certificate {
    #[n(1)] block_hash: Hash32,
    #[n(2)] round: i32,
    #[n(3)] committers: Vec<i32>,
    #[n(4)] absentees: Vec<i32>,
    #[n(5)] signature: Signature,
}
```

:::

::: tab 🇬 Golang

```go
type Certificate struct {
   BlockHash  Hash      `cbor:"1,keyasint"`
   Round      int       `cbor:"2,keyasint"`
   Committers []int     `cbor:"3,keyasint"`
   Absentees  []int     `cbor:"4,keyasint"`
   Signature  Signature `cbor:"5,keyasint"`
}
```

:::

::::

## Certificate Verification

For each round validators cast their votes by signing `BlockHash | Round`. A valid certificate
should have signature from validators with more than 2/3 of the stake for that round. If enough
votes are collected, the signatures will be aggregated into one single signature. To verify the
aggregated signature, from `Committers` and `Absentees` the aggregated public key can be formed and
therefore Signature can be verified.

## Example

Hers is an example of a certificate data.

<hexdump bytes="a50158200ca12eee3c791ba4b78439448d59a4b817d1eaec10aa090ea40f9af3d43e6e2b020003840001020304800558307b4ddaeb4502b544790f73bd4d3de51e7094192cb356bb3f6ac898f46ab24779497cd3226a6025f81c5b56474a5cbd84" />

Which can be interpreted in
[CBOR](http://cbor.me/?bytes=a50158200ca12eee3c791ba4b78439448d59a4b817d1eaec10aa090ea40f9af3d43e6e2b020003840001020304800558307b4ddaeb4502b544790f73bd4d3de51e7094192cb356bb3f6ac898f46ab24779497cd3226a6025f81c5b56474a5cbd84)
format:

```
{
  1: h'0CA12EEE3C791BA4B78439448D59A4B817D1EAEC10AA090EA40F9AF3D43E6E2B',
  2: 0,
  3: [0, 1, 2, 3],
  4: [],
  5: h'7B4DDAEB4502B544790F73BD4D3DE51E7094192CB356BB3F6AC898F46AB24779497CD3226A6025F81C5B56474A5CBD84'
}
```

Certificate hash is the hash of the certificate in binary format. For this example, the certificate
hash is:

```
0x74fabc0be964b5373800026923b1bb66bf4042d6856396511aebe083c773ee9a
```
