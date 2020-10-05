# Unikname DID Specification #



## Author ##

Space Elephant // Unikname // **uns.network**

TODO: Choose Author



## Abstract ##

TODO: Modify abstract



As the world is turning increasingly more digital, there is a growing need for globally unique identifiers. Decentralized Identifiers (DIDs) offer an alternative to traditional solutions relying on central entities to issue and manage such identifiers. DIDs take advantage of decentralized verifiable data registry such as blockchains to enable the decentralization of these processes and give users control over their own online identities. 

This document defines the `unik` DID method in compliance with the W3C's [DID Specification](https://w3c.github.io/did-core/). It describes how unik-specific DID are generated as well as how to manage the corresponding DIDs and resolve them to DID Documents (DDoc). `unik` DID cannot be their own controller. `unik` DID are build to work with `uns` DID [ADD LINK] but are compatible with any other type of DIDs.

This DID method has been registered in the [DID Specification Registries](https://w3c.github.io/did-spec-registries/#did-methods).



## Intro ##

TODO: Write intro

What we do

human-readable blockchain-based decentralized

DID associated to these @uniknames.

This is to resolve DID to DID documents

To solve @unikname to DID and more, Unikname has developed a resolver



## Target System ##

The `unik` DID method uses [**uns.network**](docs.uns.network) as the underlying Verifiable Data Registry.

**uns.network** is a distributed network and protocol dedicated to handling IDs rooted in the blockchain, aiming to secure any web and mobile connections, and to protect users' privacy. 



TODO: Present uns.network

Based on Ark.io

Info + How to participate [link]



### Tools ###

When working with **uns.network**, there are two preferred tools: a command line tool, and a graphical interface. 

#### Command Line Interface (CLI) ####

**uns.network** provides an interactive [Command Line Interface](https://docs.uns.network/uns-use-the-network/cli.html#download-and-installation) to create and manage @unikname as well as their properties and the cryptoaccount that own them. The rest of this specification will provide examples using the CLI. 

#### My Unikname app ###

PRESENT APP HERE



## DID Method Name ##

This method is identified by the following string:  `unik`

A DID that uses this method MUST begin with the following prefix: `did:unik:`

The prefix MUST be in lowercase as per the [DID specification](https://www.w3.org/TR/did-core/#did-syntax). The format and generation of method-specific identifiers are defined below.



## Method-specific Identifier ##

A `unik` DID identifies a @unikname. A @unikname is a Non Fungible Token (NFT) from the **uns.network** that INSERT DESCRIPTION HERE.

As such, this method uses the unik-id, the primary key of said NFT, as its identifier. 

 A `unik` DID is formated as follows:

```
unik-did = "did:unik:" uns-specific-idstring
unik-specific-idstring = unik-id
unik-id = 64*HEXDIG
```



## Example DID document ##

We provide an example DID document below:

```json
{
    "@context": ["https://www.w3.org/ns/did/v1"],
    "id": "did:unik:158cffbe4d7b567468a17290c0cd1546ea3b013059a3a471e5ad309cfddfb0e3",
    "created": "2020-04-28T04:38:08.000Z",
    "controller": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
    "publicKey": [
        {
            "id": "did:unik:158cffbe4d7b567468a17290c0cd1546ea3b013059a3a471e5ad309cfddfb0e3#controller",
            "controller": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "publicKeyHex": "02ef4ee8587a532cfb3bf3527c9ad1c14405cb1c6638ea477c5177dc121e35ea67"
        }
    ], 
	"authentication": ["controller"]
}
```

#### Required/Mandatory fields ####

##### id #####

As per the DID Specification, all DID Document MUST contain an id field.

##### controller #####

A `unik` DID Document MUST include a controller field. This field cannot point to a `unik` DID.

At the moment, `unik` DID only support `uns` DID controllers. This may be extended in the future.

##### publicKey #####

A `unik` DID Document MUST include a list of public keys, at least one of which MUST belong to their controller. Such a key is labeled as `#controller`.

##### authentication #####

SHOULD THIS BE MANDATORY ?

A `unik` DID Document MUST include an authentication field. The default value for this field is `["controller"]`.



TODO:

- answer questions
- address remarks



Questions & Remarks :

- publicKey may be replaced by verificationMethod
- The DIF has not yet reached a consensus on key format. It is currently using PEM and Base58 (Bitcoin). Other candidates are:
  - base64url
  - base16 (hex)
  - Jwk (JSON Web Key - normative definition still pending)
- Do we want to expose service endpoints ?
- Do we want to include creation/modification date ?
- Validate that the verification method type is correct



## CRUD Operation Definitions  ##



### Create (Register) ###

The creation of a `unik` DID requires three steps: the user chooses the @unikname represented by the DID, verifies its availability, and registers it on **uns.network**.



#### Step 1 - Choose a @unikname ####

A @unikname is a human-readable identifier. It can be used to authenticate one-self on the Internet or to aggregate and advertise properties in an easily discoverable manner. @uniknames are private by default but can be publicly disclosed. 

SHOULD WE MENTION THE PATTERN ?

 A @unikname is formatted as follows:

```
@["type":]"explicitValue"
```

##### Type #####

There are 3 types of @uniknames that can be represented either by a number or a string:

| Type (string) | Type (numerical value) | Scope |
| :------------ | ---------------------- | ----- |
| individual    | 1                      |       |
| organization  | 2                      |       |
| network       | 3                      |       |

When no type is specified, it defaults to individual.

A @unikname therefore has several equivalent representations. For instance, the following @uniknames are equivalent:

```
@bob
@1:bob
@individual:bob
```



##### Explicit value #####

The explicit value is the human-readable part of a @unikname. It can be an arbitrary long string of characters from the Safetypo:copyright: alphabet[LINK TO SOMETHING HERE]. 

> :warning: For applicative purposes, we recommend an explicit value no longer than 100 characters.

To prevent phishing attempts, explicit values that are too close to existing @unikname cannot be registered. For instance, the following @uniknames are equivalent:

```
@bob
@BOB
@b0b
@b.o.b
@bob--------------------
```

For more information; SEE WHAT ?

IF PATTERN? ADD HERE.



#### Step 2 - Verify availability ####

For privacy's sake, @uniknames are not directly written on the chain. Instead, the **uns.network** registers a *unik-id* derived from the @unikname. It is therefore not possible to access a registry of all the existing @uniknames. However, it is easy to verify if a given @unikname is available.

 The following command can be used to read the information related to a given @unikname: 

```bash
$ uns unik:read TARGET
```

Example:

```bash
$ uns unik:read @bob -f yaml
data:
  id: 158cffbe4d7b567468a17290c0cd1546ea3b013059a3a471e5ad309cfddfb0e3
  owner:
    address: UYWaMkArHJjMecuHgs6LYapFtvV27QeafX
    balance: 9.97
    token: UNS
  creationBlock: 3af261e99286fadf83d35bf39b8a199525e4e535cd91597b23e36d296eb54d3f
  creationTransaction: 2c834a17694e55716411da681db38ab233adf71fc6e4028a58728619c70a3031
  creationDate: 2020-04-28T04:38:08.000Z
  properties:
    - explicitValues: bob
    - usr/wallet/ark: ark:AMN48dmd3g8rgAT1xhTYfi4zwEBWpCjNDk
    - usr/wallet/btc: btc:bc1qt9qrhany5l0yn040rak4h9jcsu6v9d48sysrna
    - type: "1"
    - UnikVoucherId: ow8RSWf-5CX9_f17aK-hQ
    - LifeCycle/Status: "3"
    - Badges/Security/SecondPassphrase: "false"
    - Badges/NP/Delegate: "false"
    - Authentications/CosmicNonce: "1"
```



If the target @unikname does not exit, the following message will be returned

```bash
$ uns unik:read @lulu
» :stop: DID does not exist;
```



#### Step 3 - Register it on the chain ####

> :warning: *Pre-requisite*: You need a **uns.network** account with enough UNS (SPECIFY PRICE HERE ? LINK TO IT ?)
>
> This step writes a transaction on the **uns.network** blockchain. It requires a blockchain account with enough funds to pay for the transaction.



To claim a @unikname (and its associated DID), one must register the @unikname on **uns.network**.

Here again, the [Command Line Interface](https://docs.uns.network/uns-use-the-network/cli.html#download-and-installation) provides a method to do so:

```bash
$ uns unik:create --explicitValue {explicitValue} --type [individual|organization|network] --coupon {coupon}
```

The user will be prompted to enter their passphrase [LINK TO UNS SPEC HERE ?] to sign the transaction. This authenticates the transaction.

Example of successful output:

```bash
Computing UNIK fingerprint... done
Creating transaction... done
Sending transaction... done
Waiting for transaction confirmation... done
UNIK nft created (1 confirmation(s)): bf21b5c7ae13a6892315aefcfa58ee1b1c470d011564f9f29c4f1941a2373956 [ https://explorer.uns.network/#/uniks/bf21b5c7ae13a6892315aefcfa58ee1b1c470d011564f9f29c4f1941a2373956 ]
See transaction in explorer: https://explorer.uns.network/#/transaction/a73f42691f2d076ba5a4e12c36f43ed8082cb8ae03c507d98305b8a08e6d4f03
{
  "data": {
    "id": "bf21b5c7ae13a6892315aefcfa58ee1b1c470d011564f9f29c4f1941a2373956",
    "transaction": "a73f42691f2d076ba5a4e12c36f43ed8082cb8ae03c507d98305b8a08e6d4f03",
    "confirmations": 1
  }
}
```



#### All-in-one method - The Unikname App ####

Alternatively, my Unikname App provides a graphical interface to perform all three previous steps.

ADD SCREENSHOT HERE ?

LIEN D'INSTALL ?



### Read (Resolve) ###

The DDoc is constructed by extracting information written into the **uns.network** blockchain. These operations are read-only and are therefore not permissionned. 

To retrieve information linked to a @unikname, use the following method: 

```bash
$ uns unik:read TARGET
```

Example output:

```bash
$ uns unik:read @bob -f yaml
data:
  id: 158cffbe4d7b567468a17290c0cd1546ea3b013059a3a471e5ad309cfddfb0e3
  owner:
    address: UYWaMkArHJjMecuHgs6LYapFtvV27QeafX
    balance: 9.97
    token: UNS
  creationBlock: 3af261e99286fadf83d35bf39b8a199525e4e535cd91597b23e36d296eb54d3f
  creationTransaction: 2c834a17694e55716411da681db38ab233adf71fc6e4028a58728619c70a3031
  creationDate: 2020-04-28T04:38:08.000Z
  properties:
    - explicitValues: bob
    - usr/wallet/ark: ark:AMN48dmd3g8rgAT1xhTYfi4zwEBWpCjNDk
    - usr/wallet/btc: btc:bc1qt9qrhany5l0yn040rak4h9jcsu6v9d48sysrna
    - type: "1"
    - UnikVoucherId: ow8RSWf-5CX9_f17aK-hQ
    - LifeCycle/Status: "3"
    - Badges/Security/SecondPassphrase: "false"
    - Badges/NP/Delegate: "false"
    - Authentications/CosmicNonce: "1"
```



To retrieve the public key associated with the owner's address, use the following command:

```bash
$uns cryptoaccount:read TARGET
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



The example above would yield the following DDoc:

```json
{
    "@context": ["https://www.w3.org/ns/did/v1"],
    "id": "did:unik:158cffbe4d7b567468a17290c0cd1546ea3b013059a3a471e5ad309cfddfb0e3",
    "created": "2020-04-28T04:38:08.000Z",
    "controller": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
    "publicKey": [
        {
            "id": "did:unik:158cffbe4d7b567468a17290c0cd1546ea3b013059a3a471e5ad309cfddfb0e3#controller",
            "controller": "did:uns:UYWaMkArHJjMecuHgs6LYapFtvV27QeafX",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "publicKeyHex": "02ef4ee8587a532cfb3bf3527c9ad1c14405cb1c6638ea477c5177dc121e35ea67"
        }
    ], 
	"authentication": ["controller"]
}
```



### Update ###

We distinguish between two types of updates: an ownership change, and the management of DDoc properties (addition or update). 

#### Ownership change ####

@unikname can be exchanged as any other NFT. That type of transaction is currently deactivated on **uns.network**. Support for this type of update is part of our roadmap and will be included in later iterations of this DID method.



#### DDoc property management ####

For a property to appear in the DDoc, it MUST be registered as a property of the corresponding @unikname with the following syntax:

```
key = "did-" DDoc-property-name
```

The value associated to these keys MUST be a valid JSON member. An example:

```
key = did-assertionMethod
value = '["controller"]'
```

This will add the following line to the DDoc:

```json
	"assertionMethod": ["controller"]
```



The following properties are excluded from this requirement:

- @context
- id
- controller
- created
- publicKey
- authentication



Two methods can be used to manage @unikname's properties, `properties:set` and `properties:unset`, that add/update and remove properties respectively:

```bash
$ uns properties:set {TARGET} --key "{key1}" --value "{value1}" --key "{key2}" --value "{value2}"
```

```bash
$ uns properties:unset {TARGET} -k prop1 -k prop2
```

> :warning: using the CLI, the value is limited to a 255-character long string.



##### Unauthorized properties #####

The following properties CANNOT be set in this way. When they appear as a @unikname properties, they MUST be disregarded when building the DDoc.

- did-id
- did-controller
- created



### Delete (Deactivate) ###

This function in not yet supported by the `unik` DID method.

BURN INCLUDED LATER



## Reference Implementation ##

None for now

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

