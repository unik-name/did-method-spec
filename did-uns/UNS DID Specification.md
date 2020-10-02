# Uns DID Specification #



## Author ##

Space Elephant // Unikname // **uns.network**

TODO: Choose Author



## Abstract ##

As the world is turning increasingly more digital, there is a growing need for globally unique identifiers. Decentralized Identifiers (DIDs) offer an alternative to traditional solutions relying on central entities to issue and manage such identifiers. DIDs take advantage of decentralized verifiable data registry such as blockchains to enable the decentralization of these processes and give users control over their own online identities. 

This document defines the `uns` DID method in compliance with the W3C's [DID Specification](https://w3c.github.io/did-core/). It describes how uns-specific DID are generated as well as how to manage the corresponding DIDs and resolve them to DID Documents.

This DID method has been registered in the [DID Specification Registries](https://w3c.github.io/did-spec-registries/#did-methods).



## Intro ##

TODO: Write intro ?



## Target System ##

The `uns` DID method uses [**uns.network**](docs.uns.network) as the underlying Verifiable Data Registry.

**uns.network** is a distributed network and protocol dedicated to handling IDs rooted in the blockchain, aiming to secure any web and mobile connections, and to protect users' privacy. 



COMMENT: WHAT DO WE WANT TO SAY HERE?



## DID Method Name ##

This method is identified by the following string:  `uns`

A DID that uses this method MUST begin with the following prefix: `did:uns:`

The prefix MUST be in lowercase as per the [DID specification](https://www.w3.org/TR/did-core/#did-syntax). The format and generation of method-specific identifiers are defined below.



## Method-specific Identifier ##

This method uses **uns.network** addresses as identifiers. The DID identifies the holder of the private key associated with the address. A `uns` DID is formated as follows:

```
uns-did = "did:uns" uns-specific-idstring
uns-specific-idstring = uns-address
```

> :warning: **uns.network** addresses ARE case-sensitives.

**uns.network** is based on [Ark.io](https://ark.io/). As such, **uns.network** addresses follow the generation pattern specified in [Ark.io documentation](https://learn.ark.dev/concepts/cryptography):

| <img src="passphrase_account.png" alt="passphrase_account.png" style="zoom:50%;" /> |
| :----------------------------------------------------------: |
|                *Uns Address Generation Steps*                |



Different prefixes are used to identify different networks. **uns.network** uses the following prefixes:

| Network | dec  | hex  | Prefix | Example address                    |
| :------ | ---- | :--: | :----: | ---------------------------------- |
| Livenet | 68   | 0x44 |   U    | UYWaMkArHJjMecuHgs6LYapFtvV27QeafX |
| Sandbox | 63   | 0x3F |   S    | SMoCXZbMHTBLFtg15GnRYuFyUFk2gt44zb |



The uniqueness and security of a **uns.network** crypto-account (and therefore those of the associated DID) depend on the randomness of the associated passphrase. The cryptographic properties of the functions used to derive the address from the passphrase (ie. non reversibility and collusion resistance) and the security of the underlying elliptic curve (secp256k1) ensure that the private key cannot be recovered from the address or public key. But if the passphrase can be guessed, the crypto-account is compromised.



## Example uns DID Document (DDoc) ##

We provide an example DID Document below:

```json
{
    "@context": ["https://www.w3.org/ns/did/v1"],
    "id": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
    "created": "2020-04-28T06:38:08Z",
    "publicKey": [
        {
            "id": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX#vm1",
            "controller": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "publicKeyBase58": "Kmp7t8E7tK9Buj2r8z2wjMbSYMzPunQ1j6h25jn5oRFyUDX66n4W32GN7ohhgFNhGkNJErRcnS1hGP7Ueb3vLYjfdQ"
        }
    ], 
	"authentication": ["vm1"]
}
```



## CRUD Operation Definitions  ##

### Create (Register) ###

#### Step 1 - Private Key generation ####

The first step in creating a `uns` DID is to generate a **uns.network** address, which requires a passphrase that will be derived into a public/private key pair. 

> :warning: **The security of one's account is dependent on the associated passphrase randomness**: 
>
> One can technically use any word, phrase, or string as a passphrase which will result in a valid **uns.network** cryptoaccount; however, it is heavily discouraged as the security of an address relies on the randomness of its passphrase. Humans are bad at creating randomness, and entering sequences of random letters and numbers isn't easy to do accurately.
>
> **uns.network** passphrases are implemented using ARK.IO algorithms, based on the BIP39 Protocol, and are a combination of twelve words.



The creation of the private key is implicitly equivalent to the creation of the associated DID. This step can be achieved independently from the network.  

**uns.network** provides an interactive [Command Line Interface](https://docs.uns.network/uns-use-the-network/cli.html#download-and-installation) to create and manage crypto accounts. The following command can be used for account creation: 

```bash
$ uns cryptoaccount:create
```

Example output:

```bash
$ uns cryptoaccount:create

⚠️  WARNING: this information is not saved anywhere. You need to copy and save it by your own. ⚠️

{
  "address": "U9B6HLADr1Fd6TXvEGCuNc3A9aHK9JzjYC",
  "publicKey": "03522706bd0b812faea10e92dc0400e37aba468f9df3e2f63570c11c2b66eadc22",
  "privateKey": "d7877b7867404cd35bf85ea7643ad23058f5af1262d11a389ffc429648f4abe7",
  "passphrase": "train drastic alley office seed glove cable fee firm during lottery cause",
  "network": "livenet"
}
```

For more information, see the [relevant documentation](https://docs.uns.network/uns-use-the-network/cli.html#cryptoaccount-create).



#### Step 2: Address diclosure ####

**uns.network** addresses need not be registered with the network to be valid. However, if **uns.network** has no record involving a given address, the  associated DDoc cannot be generated. Therefore, to complete the DID creation process, the private key holder MUST perform an operation on the **uns.network**.



### Read (Resolve) ###

The DDoc is constructed by extracting information written into the **uns.network** blockchain. These operations are read-only and are therefore not permissionned.  **uns.network** provides an interactive [Command Line Interface](https://docs.uns.network/uns-use-the-network/cli.html#download-and-installation). The following command can be used to read account information: 

```bash
$ uns cryptoaccount:read TARGET 
```

Example output:

```bash
$ uns cryptoaccount:read UYWaMkArHJjMecuHgs6LYapFtvV27QeafX

{
  "data": {
    "address": "UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
    "publicKey": "02ef4ee8587a532cfb3bf3527c9ad1c14405cb1c6638ea477c5177dc121e35ea67",
    "balance": 9.97,
    "token": "uns",
    "isDelegate": false,
    "nfts": {
      "unik": 1
    }
  }
```

For more information on the Command Line Interface functions, see the [relevant documentation](https://docs.uns.network/uns-use-the-network/cli.html#cryptoaccount-read).



The example output above constructs the following DDoc:

 

```json
{
    "@context": ["https://www.w3.org/ns/did/v1"],
    "id": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
    "publicKey": [
        {
            "id": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX#key1",
            "controller": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "publicKeyHex": "02ef4ee8587a532cfb3bf3527c9ad1c14405cb1c6638ea477c5177dc121e35ea67"
        }
    ]
}
```



### Update ###

This function in not supported by the `uns` DID method.



### Delete (Deactivate) ###

This function in not supported by the `uns` DID method.



## Security Considerations ##

This section follows requirements from [RFC 3552 - Section 5](https://tools.ietf.org/html/rfc3552#section-5) and the [DID Core Specification - Section 7.3](https://www.w3.org/TR/did-core/#security-requirements).

Attacks to be considered:

- eavesdropping
- replay
- message insertion
- deletion
- modification
- man-in-the-middle
- denial of service



What to say about them:

- Out of scope attacks (and why)
- In scope attacks
  - susceptible to them
  - protected against
- residual risks after threat mitigations
- if authentication
  - base assumption of why this is secure (ie, public key crypto assumptions + private key safekeeping)



For all methods defined in DID core spec section 7.2:

- Integrity Protection
- Update Authentication 



For service endpoints the following MUST be discussed :

- Method-specific endpoint authentication 
- Security assumptions based on topology (light node if any)



If the protocol incorporates cryptographic protection mechanisms, the [DID method](https://www.w3.org/TR/did-core/#dfn-did-methods) specification *MUST* clearly indicate which portions of the data are protected and what the protections are, and *SHOULD* give an indication to what sorts of attacks the cryptographic protection is susceptible. For example, integrity only, confidentiality, endpoint authentication, and so on.

## Privacy Considerations ##

This section follow specifications from [RFC6973 - Section 5](https://tools.ietf.org/html/rfc6973#section-5) and the [DID Core Specification - Section 7.4](https://www.w3.org/TR/did-core/#privacy-requirements).

At minima, this section must point to [DID Core - Section 10](https://www.w3.org/TR/did-core/#privacy-considerations).

If they apply, the spec MUST discuss the following

- surveillance
- stored data compromise
- unsolicited traffic
- misattribution
- correlation
- identification
- secondary use
- disclosure
- exclusion

## References ##

[RFC3552]	[Guidelines for Writing RFC Text on Security Considerations](https://tools.ietf.org/html/rfc3552). E. Rescorla; B. Korver. IETF. July 2003. Best Current Practice. URL: https://tools.ietf.org/html/rfc3552

[]	[ECDSA Secp256k1 Signature 2019](https://w3c-ccg.github.io/lds-ecdsa-secp256k1-2019/). O. Steele. W3C. April 2019

[]	[Linked Data Proofs](https://w3c-ccg.github.io/ld-proofs/). D. Longley, M. Sporny. W3C. 

