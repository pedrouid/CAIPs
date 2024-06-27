---
caip: 300
title: Wallet Authenticate JSON-RPC Method
author: Lukas Rosario (@lukasrosario), Conner Swenberg (@ilikesymmetry), Pedro Gomes (@pedrouid), Luka Isailovic (@lukaisailovic)
discussions-to: https://github.com/ChainAgnostic/CAIPs/discussions/300
status: Draft
type: Standard
created: 2023-06-28
requires: 2, 10,
---

## Simple Summary

This CAIP defines a JSON-RPC method to request a batch of methods when connecting a wallet to resolve in a single roundtrip.

## Abstract

This proposal enables a single-click experience to resolve several RPC requests during the connection approval when it's established when the user is requested to connect its wallet. It gives the ability for a wallet to not only establish a session, but also authenticate the user, expose capabilities or features, define some onchain permissions, etc.

## Motivation

TODO

## Specification

This JSON-RPC method can be requested to a wallet provider without prior knowledge of any blockchain accounts, chains, methods or other features.

### Request

The application would interface with a wallet to make request as follows:

```jsonc
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "wallet_connect",
  "params": {
      "requests?": {
        "method": string,
        "params": unknown
      }[],
  }
}
```

The JSON-RPC method is labelled as `wallet_connect` and expects the following parameters:

- requests - is an OPTIONAL array of rpc requests that would be batched togehter when requesting the user for approval
  - method - the RPC method being requested
  - params - will include params specific to the RPC method

### Response

The wallet will prompt the user with a dedicated UI to display the app requesting the authentication and allow the user to select which blockchain account to sign with.

#### Success

If approved, the wallet will return a list of signed, valid CACAOs for each account authorized on the networks requested by the `chains` property.

```jsonc
{
  "id": 1,
  "jsonrpc": "2.0",
  "result": { "result": unknown } || { error: { message: string, code: number } }[]
}
```

The JSON-RPC response will include an array of requests which are going to be ordered in the same order as provided in the request.

#### Failure

Request will fail if rejected by the user or if parameters fail validation.

The following Error responses MUST be used:

- User Rejected Request
  - code = 7000
  - message = "User Rejected Connect"
- Invalid Request Params
  - code = 7001
  - message = "Invalid Method Requested"

## Rationale

TODO

## Test Cases

TODO

## Security Considerations

TODO

## Privacy Considerations

TODO

## Backwards Compatibility

TODO

## Links

- [CAIP-2][caip-2] - Blockchain ID Specification
- [CAIP-10][caip-10] - Account ID Specification

[caip-2]: https://chainagnostic.org/CAIPs/caip-2
[caip-10]: https://chainagnostic.org/CAIPs/caip-10

## Copyright

Copyright and related rights waived via [CC0](../LICENSE).
