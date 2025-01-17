---
id: walletconnect-json-rpc-methods
title: WalletConnect JSON-RPC Methods
---

[comment]: # (mx-abstract)

The WalletConnect [Sign API](https://specs.walletconnect.com/2.0/specs/clients/sign) establishes a session between a dapp and a wallet in order to expose a set of blockchain accounts that can sign transactions and/or messages using a secure remote JSON-RPC transport with methods and events.
The following methods are already implemented in [xPortal](/wallet/xportal/) and in WalletConnect's [Web Examples](https://github.com/WalletConnect/web-examples).

- [React Wallet (Sign v2)](https://github.com/WalletConnect/web-examples/tree/main/wallets/react-wallet-v2) ([Demo](https://react-wallet.walletconnect.com/))
- [React dApp (with standalone client) - v2](https://github.com/WalletConnect/web-examples/tree/main/dapps/react-dapp-v2) ([Demo](https://react-app.walletconnect.com/))

The Transaction Object is the same for both the `mvx_signTransaction` and the `mvx_signTransactions` methods.
To get the transaction object into a ready-to-serialize, plain JavaScript object, one can use `.toPlainObject()` from `@multiversx/sdk-core` or [any other available SDKs](/sdk-and-tools/overview).

[comment]: # (mx-context-auto)

## mvx_signTransactions

Sign a list of transactions.
This method returns a signature and any additional properties ( e.g. Guardian info ) that must be applied to each of the provided Transactions before broadcasting them on the network.

### Parameters

    1. `Object` - Signing parameters:
        1.1. `transactions` : `Array<Object>` - Array of Transactions
            1.1.1 `Object` - Transaction Object
                1.1.1.1  `nonce` : `String` - The Nonce of the Sender.
                1.1.1.2  `value` : `String` - The Value to transfer, as a string representation of a Big Integer (can be "0").
                1.1.1.3  `receiver` : `String` - The Address (bech32) of the Receiver.
                1.1.1.4  `sender` : `String` - The Address (bech32) of the Sender.
                1.1.1.5  `gasPrice` : `Number` - The desired Gas Price (per Gas Unit).
                1.1.1.6  `gasLimit` : `Number` -  The maximum amount of Gas Units to consume.
                1.1.1.7  `data` : `String | undefined` -  The base64 string representation of the Transaction's message (data).
                1.1.1.8  `chainID` : `String` - The Chain identifier. ( `1` for Mainnet, `T` for Testnet, `D` for Devnet )
                1.1.1.9  `version` : `String | undefined` -  The Version of the Transaction (e.g. 1).
                1.1.1.10 `options` : `String | undefined` -  The Options of the Transaction (e.g. 1).
                1.1.1.11 `guardian` : `String | undefined` -  The Address (bech32) of the Guardian.
                1.1.1.12 `receiverUsername` : `String | undefined` - The base64 string representation of the Sender's username.
                1.1.1.10 `senderUsername` : `String | undefined` - The base64 string representation of the Receiver's username.

### Returns

    1. `Object`
        1.1. `signatures` : `Array<Object>`
            1.1.1 `Object` - corresponding signature and optional properties response for the provided transaction
                1.1.1.1 `signature` :  The Signature (hex-encoded) of the Transaction.
                1.1.1.2 `guardian` : `String | undefined` - The Address (bech32) of the Guardian.
                1.1.1.3 `guardianSignature` : `String | undefined` -  The Guardian's Signature (hex-encoded) of the Transaction.
                1.1.1.4 `options` : `Number | undefined` -  The Version of the Transaction (e.g. 1).
                1.1.1.5 `version` : `Number | undefined` -  The Options of the Transaction (e.g. 1).

### Example

```javascript
// Request
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "mvx_signTransactions",
    "params": {
        "transactions": [
            {
                "nonce": 42,
                "value": "100000000000000000",
                "receiver": "erd1cux02zersde0l7hhklzhywcxk4u9n4py5tdxyx7vrvhnza2r4gmq4vw35r",
                "sender": "erd1uapegx64zk6yxa9kxd2ujskkykdnvzlla47uawh7sh0rhwx6y60sv68me9",
                "gasPrice": 1000000000,
                "gasLimit": 70000,
                "data": "Zm9vZCBmb3IgY2F0cw==", // base64 representation of "food for cats"
                "chainID": "1",
                "version": 1
            },
            {
                "nonce": 43,
                "value": "300000000000000000",
                "receiver": "erd1ylzm22ngxl2tspgvwm0yth2myr6dx9avtx83zpxpu7rhxw4qltzs9tmjm9",
                "sender": "erd1uapegx64zk6yxa9kxd2ujskkykdnvzlla47uawh7sh0rhwx6y60sv68me9",
                "gasPrice": 1000000000,
                "gasLimit": 70000,
                "data": "Zm9vZCBmb3IgZG9ncw==", // base64 representation of "food for dogs"
                "chainID": "1",
                "version": 1
            }
        ]
    }
}

// Result
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
        "signatures": [
            {
                "signature": "1aa6cdd9f614e2a1cedcc207e6e7c574674c9b05e98f31035cac89fcca2673ca9273c48823418cf44696f64a2c535ab3784f680a0c6d6e84b960c33e586cb30b"
            },
            {
                "signature": "43127c0ac3d5b124ced9c15e884940fb3c1256c463a74db33c1842fa323971e1f43725eea62225c6b2f9b2634edf68ad2e315241df734d60c41b920dec85b60a"
            }
        ]
    }
}
```

[comment]: # (mx-context-auto)

## mvx_signTransaction

This method returns a signature and any additional properties ( e.g. Guardian info ) that must be applied to the transaction before broadcasting it on the network.
Similar to `mvx_signTransactions`, but only one Transaction can be signed at a time instead of a list of transactions.
The same logic applies to the Transaction Object here too.

### Parameters

    1. `Object` - Signing parameters:
        1.1. `transaction` : `Object` - Transaction Object
            1.1.1  `nonce` : `String` - The Nonce of the Sender.
            1.1.2  `value` : `String` - The Value to transfer, as a string representation of a Big Integer (can be "0").
            1.1.3  `receiver` : `String` - The Address (bech32) of the Receiver.
            1.1.4  `sender` : `String` - The Address (bech32) of the Sender.
            1.1.5  `gasPrice` : `Number` - The desired Gas Price (per Gas Unit).
            1.1.6  `gasLimit` : `Number` -  The maximum amount of Gas Units to consume.
            1.1.7  `data` : `String | undefined` -  The base64 string representation of the Transaction's message (data).
            1.1.8  `chainID` : `String` - The Chain identifier. ( `1` for Mainnet, `T` for Testnet, `D` for Devnet )
            1.1.9  `version` : `String | undefined` -  The Version of the Transaction (e.g. 1).
            1.1.10 `options` : `String | undefined` -  The Options of the Transaction (e.g. 1).
            1.1.11 `guardian` : `String | undefined` -  The Address (bech32) of the Guardian.
            1.1.12 `receiverUsername` : `String | undefined` - The base64 string representation of the Sender's username.
            1.1.10 `senderUsername` : `String | undefined` - The base64 string representation of the Receiver's username.

### Returns

    1. `Object` - corresponding signature and optional properties response for the provided transaction
        1.1 `signature` :  The Signature (hex-encoded) of the Transaction.
        1.2 `guardian` : `String | undefined` - The Address (bech32) of the Guardian.
        1.3 `guardianSignature` : `String | undefined` -  The Guardian's Signature (hex-encoded) of the Transaction.
        1.4 `options` : `Number | undefined` -  The Version of the Transaction (e.g. 1).
        1.5 `version` : `Number | undefined` -  The Options of the Transaction (e.g. 1).

### Example

```javascript
// Request
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "mvx_signTransaction",
    "params": {
        "transaction": {
            "nonce": 42,
            "value": "100000000000000000",
            "receiver": "erd1cux02zersde0l7hhklzhywcxk4u9n4py5tdxyx7vrvhnza2r4gmq4vw35r",
            "sender": "erd1ylzm22ngxl2tspgvwm0yth2myr6dx9avtx83zpxpu7rhxw4qltzs9tmjm9",
            "gasPrice": 1000000000,
            "gasLimit": 70000,
            "data": "Zm9vZCBmb3IgY2F0cw==", // base64 representation of "food for cats"
            "chainID": "1",
            "version": 1
        }
    }
}

// Result
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
        "signature": "5845301de8ca3a8576166fb3b7dd25124868ce54b07eec7022ae3ffd8d4629540dbb7d0ceed9455a259695e2665db614828728d0f9b0fb1cc46c07dd669d2f0e"
    }
}
```

[comment]: # (mx-context-auto)

## mvx_signMessage

This method returns a signature for the provided message from the requested signer address.

### Parameters

    1. `Object` - Signing parameters:
        1.1. `message` : `String` -  the message to be signed
        1.2. `address` : `String` -  bech32 formatted MultiversX address ( erd1... )

### Returns

    1. `Object`
        1.1. `signature` : `String` - corresponding signature for the signed message

### Example

```javascript
// Request
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "mvx_signMessage",
    "params": {
        "message": "food for cats",
        "address": "erd1uapegx64zk6yxa9kxd2ujskkykdnvzlla47uawh7sh0rhwx6y60sv68me9"
    }
}

// Result
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
        "signature": "513fb2fa5ac39282ffc3aa90a89024b77057ac4542199673b05601302668bdda36c1076952f4c7445f4c6487a4263d51f72dff325012ab3f236594546ef54408"
    }
}
```

## mvx_signNativeAuthToken

A dApp (and its backend) might want to reliably assign an off-chain user identity to a MultiversX address. On this purpose, the signing providers allow a login token to be used within the login flow - this token is signed using the wallet of the user. Afterwards, a backend application would normally [verify the signature](/sdk-and-tools/sdk-js/sdk-js-signing-providers/#verifying-the-signature-of-a-login-token) of the token.

The functionality is mostly the same as `mvx_signMessage`, only in this case instead of signing the provided message, the wallet will sign a special format including the requested signer address and the provided login token in the form of `${address}${token}`.

### Parameters

    1. `Object` - Signing parameters:
        1.1. `token` : `String` -  the loginToken to be signed
        1.2. `address` : `String` -  bech32 formatted MultiversX address ( erd1... )

### Returns

    1. `Object`
        1.1. `signature` : `String` - corresponding signature for the signed token

### Example

```javascript
// Request
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "mvx_signNativeAuthToken",
    "params": {
        "token": "aHR0cHM6Ly9kZXZuZXQueGV4Y2hhbmdlLmNvbQ.c6191feb77da75e1acb3c5c3e8d4053be370d925fe7a78c7958ff5edc63d0c8c.86400.eyJ0aW1lc3RhbXAiOjE2OTM3NjQ1ODh9",
        "address": "erd1uapegx64zk6yxa9kxd2ujskkykdnvzlla47uawh7sh0rhwx6y60sv68me9"
    }
}

// Result
{
    "id": 1,
    "jsonrpc": "2.0",
    "result": {
        "signature": "2789172fd8e0f3b81767392b4f3450807a5894e5c704073d18a0d5e4d0819cd8fac53ef8ba3e3b0430481d6e396f67ae484ae1f295befa766e49a3abfdf76e0a"
    }
}
```

[comment]: # (mx-context-auto)

## mvx_signLoginToken

Exactly the same functionality as `mvx_signNativeAuthToken`, only the login token format differs. The Wallet can display a different UI based on the login token method request.

## mvx_cancelAction

Wallets can implement this method to improve the UX. It is used to transmit that the user wishes to renounce on a triggered action. Close a sign transaction modal or a sign message modal, etc.

### Parameters

    1. `Object` - Action parameters
        1.1. `action` : `String | undefined` -  Current action to be cancelled ( for ex. `cancelSignTx` )


### Returns

    void

### Example

```javascript
// Request
{
    "id": 1,
    "jsonrpc": "2.0",
    "method": "mvx_cancelAction",
    "params": {
        "action": "cancelSignTx"
    }
}
```
