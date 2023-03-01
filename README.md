
## Introduction
`did:kaname` is designed to uniquely identify a subject within the [Kaname Protocol](https://kaname.io).

### Kaname Protocol
Kaname Protocol is a communication convention that facilitates a win-win relationship between project owners and collaborators in various kinds of projects.  More information is in [official site](https://kaname.io).

## Syntax and Interpretation

```
# Kaname DID
did:kaname:<method-specific-id>
```

```
did-kaname:       "did:kaname:" + namespace? + ":" + member? 
member:    　　　　　　　　　　  address
namespace:        [0-9]{1,64} | address
adderss:          according to [CAIP-10]
```    

for examples:
```did
did:kaname:eip155:1:0x4Ac5EC62CeA97De5cF3a58BD9EF41FfAe911363D:eip155:1:0xDB10E4a083B87e803594c12c679422dCe5FCCCB9:
did:kaname:eip155:1:0x4Ac5EC62CeA97De5cF3a58BD9EF41FfAe911363D:
did:kaname::eip155:1:0xDB10E4a083B87e803594c12c679422dCe5FCCCB9:
did:kaname:message:eip155:1:0xDB10E4a083B87e803594c12c679422dCe5FCCCB9:
```

Its corresponding DID document is as follows:
```
{
  "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://kaname.io/did/v1",
      {
          "EcdsaSecp256k1RecoveryMethod2020": 
      }
  ],
  "id": ${did},
  "verificationMethod": [
     {
        id: "${did}#controller",
        type: "EcdsaSecp256k1RecoveryMethod2020", 
        controller: ${did}
     },
     {
        id: "${did}#delegate",
        type: "EcdsaSecp256k1RecoveryMethod2020", 
        controller: ${did}
     },
  ],
  "authentication": ["#controller", "#delegate"],
  "assertionMethod": ["#controller", "#delegate"], 
  "service": [
    {
      "id": "#questry",
      "type": "questry",
      "serviceEndpoint": "https://questry.io/mypage/mizuki"
    }
   ]
}
```

## DID Operations

### Create
Since the DID itself is tied to a smart contract on any chains, theIt is automatically created.
- Project Contract Addr: `0xABcD...CCB8` => `did:kaname:eip155:1:0xABcD...CCB8:`
- Member Addr: `0x123A...89BC` => `did:kaname::eip155:1:0x123A...89BC`

### Resolve (Read)

Kaname Foundation provides Public API for resolving did document,
when this API receives a resolve request, it looks up the chain and fetch if it exists.
In addition to that, we can retrieve data  by directly accessing Contract.  
if did is `did:kaname:0000:1111` , run this command
`curl https://kaname.io/did/v1/read/did:kaname:0000:1111` And you'll get the result as below
```json
{
 "did": "did:kaname:0000:1111",
 "document": {
     "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://kaname.io/did/v1"
     ],
     "id": "did:kaname:0000:1111",
     "versionCode": "1",
     "version": "1",
     "created": "2023-03-01T00:01:12.723Z",
     "updated": "2023-03-01T00:01:12.723Z",
     "publicKey": [
         {
             "id": "did:kaname:0000:1111#key-1",
             "type": "Secp256k1",
             "publicKeyHex": "0000ac12ba13267890abcdc0000ac12ba13267890abcdc0000ac12ba13267890abcdc0000ac12ba13267890abcdc0000ac12ba13267890abcdc"
         }
     ],
     "authentication": [
         "did:kaname:0000:1111#key-1"
     ],
     "recovery": [
         "did:kaname:0000:1111#key-2"
     ],
     "proof": {
         "type": "Secp256k1",
         "creator": "did:kaname:0000:1111#key-1",
         "signatureValue": "0000ac12ba13267890abcdc0000ac12ba13267890abcdc0000ac12ba13267890abcdc0000ac12ba13267890abcdc0000ac12ba13267890abcdc"
     }
 }
}
```

### Update (Replace)
Currently, this DID Document does not support update.

### Deactive (Revoke)
Currently, this DID Document does not support deactive.
    
## Security and Privacy Considerations

### Security
DID document are deployed on EVM-compatible blockchains as on-chain data, and all write/update operations must be signed with the private key corresponding to the public key of the DID controllers.

The DID document is protected by the security mechanism of the blockchain, which tamper with document, deletion, and some attacks impossible and can only be modified by one of the controllers with the associated private key. 


### Privacy Considerations
DID Documents of `did:kaname` contain no PII (Personally-Identifiable Information) only contract address and member id like a wallet address. 
And Documents is on some EVM-compatible chain. This ensures that no message insertion or modification is possible and authenticity of the DID Documents is ensured.

## Disclosure
All DID document and its historical data are stored in the some blockchain. In general, member should understand that on-chain data is public and does not have limitations on its use or disclosure.

## Update History
