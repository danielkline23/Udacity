# Star Registry - A Private Blockchain Application
Register ownership of stars. 
An app created for the Udacity Blockchain Developer Nanodegree.

## Instructions
To get the blockchain up and running, and interact with it:

0. [install Node.JS](https://nodejs.org/en/download/package-manager/), download this [repo](https://stackoverflow.com/questions/6466945/fastest-way-to-download-a-github-project), & navigate to the project directory in your terminal

1. run `npm install` to install all dependencies

2. run `node app.js` to create the genesis block & get the blockchain running on http://localhost:8000/

3. use a service like [Postman](https://learning.getpostman.com/docs/postman/launching_postman/installation_and_updates/) to send HTTP requests to the web server on port 8000
    
## I. GET /block/0 to request the Genesis Block

Take a look at the Genesis Block created by the application upon initialization.

![Request: http://localhost:8000/block/0 ](https://s3.eu-central-1.amazonaws.com/udacity-mauricemauser/get-genesis.png)


## II. POST /requestValidation for the app to prepare you a Message

Request the application to send you a Message to be signed with your Wallet, in order to prove that you are the owner of your wallet address. Include your wallet `address` in a JSON object in the request body.

The app responds with a message of this format: `<WALLET_ADRESS>:${new Date().getTime().toString().slice(0,-3)}:starRegistry`;


![Request: http://localhost:8000/requestValidation ](https://s3.eu-central-1.amazonaws.com/udacity-mauricemauser/post-request-validation.png)


## III. Sign the Message from your local Bitcoin wallet

Sign the Message with your Electrum or Bitcoin Core wallet.

![Request: http://localhost:8000/requestValidation ](https://s3.eu-central-1.amazonaws.com/udacity-mauricemauser/wallet-signature.png)


## IV. POST /submitstar to register your star

Register your star by submitting a `POST` request to `/submitstar`. Include your `wallet address`, `message`, `signature`, and the `star` object in a JSON object in the request body.
Use the following format to register a star:

```json
"star": {
    "dec": "68° 52' 56.9",
    "ra": "16h 29m 1.0s",
    "story": "Testing the story 4"
}
```

![Request: http://localhost:8000/submitstar](https://s3.eu-central-1.amazonaws.com/udacity-mauricemauser/submit-star.png)


## V. GET /blocks/<YOUR_WALLET_ADDRESS> to retrieve your stars

Request a list of all the stars your wallet `address` has stored on the blockchain by sending a `GET` request to `/blocks/<WALLET_ADDRESS>`.

![Request: http://localhost:8000/blocks/<WALLET_ADDRESS>](https://s3.eu-central-1.amazonaws.com/udacity-mauricemauser/get-stars-by-owner.png)



## Concepts
    * Block => time-stamped data, hex-encoded
    * Blockchain => linked list of blocks referencing hash of previous block
    * Wallet => public/private key pair to send to and from
    * Blockchain Identity => owner's wallet address recorded in block
    * Proof of Existence => signed message included in blockchain block

## Uses
    * Node.JS - as a JavaScript Web Server
    * Express - our Web Framework
    * bitcoinjs-lib & bitcoinjs-message - to verify Messages
    * body-parser - to extract data from HTTP requests
    * crypto-js - to hash arbitrary strings into fixed-256-bit-long SHA256 strings
    * hex2ascii - to hex-encode & decode
    * morgan - to log our HTTP requests
    * ES6 classes

## API

### 1. `Block` class:

1.1 `validate()`. 
```
    /**
     *  The `validate()` method returns a Promise that resolves with true, if everything is OK, 
     *  and false, in case the block has been tampered with - meaning the hash of the block has changed
     *  as a consequence of an outside attacker trying to change the values in the block.
     */
```
1.2 `getBData()`.
```
    /**
     *  Decodes and returns the data stored in the block body.
     */
```
     
### 2. `Blockchain` class:

2.1 `requestMessageOwnershipVerification(address)`
```
    /**
     * Request a Message that you can 
     * sign with your Bitcoin Wallet (Electrum, Bitcoin Core)
     * This has to be done before submitting your Block.
     * The method returns a Promise that resolves with the message to be signed
     *
     * @param {*} address 
     */
```
     
2.2 `submitStar(address, message, signature, star)`
```
    /**
     * Register your star by including it in a new Block on the chain.
     * This method resolves with the Block added or
     * rejects with an error.
     *
     * @param {*} address 
     * @param {*} message 
     * @param {*} signature 
     * @param {*} star 
     */
```
     
2.3 `getBlockByHash(hash)`
```
    /**
     * Returns a Promise that resolves with the Block
     * that matches the hash passed in as a parameter.
     * Search on the chain array for the block that has the hash.
     *
     * @param {*} hash 
     */
```
     
2.4 `getStarsByWalletAddress (address)`
```
    /**
     * Returns a Promise that resolves with the array of Stars objects 
     * that have been submitted to the chain by the wallet address passed in as parameter.
     * 
     * @param {*} address 
     */
```
     
2.5 `validateChain()`
```
    /**
     * Returns a Promise that resolves with a list of errors after validating the chain.
     */
```
