
# 🎲 RandoMina - Provable Random Numbers on Mina Blockchain 🔐

RandoMina enables verifiable random number generation on the Mina blockchain. 🔢 It utilizes Zero-Knowledge (ZK) proofs to ensure the randomness is provable, secure, and trustless. 🔐

This protocol has been developed as a part of [zkLocus](https://github.com/iluxonchik/zkLocus), which enables for authenticated, private and programmable geolocation sharing off & on-chain.

You can learn more about zkLocus at the following links:

- [Twitter/X 🐤](https://x.com/zkLocus/)
- [zkLocus Homepage 💻](https://zklocus.dev/)
- [zkLocus Whitepaper 📄](https://zklocus.dev/whitepaper/)
- [zkLocus GitHub Repository 📼](https://https://github.com/iluxonchik/zkLocus/)

## 🌟 Features

- 🔀 Provable pseudo-random number generation using ZK proofs
- 🔒 Secure and tamper-proof, leveraging Mina's decentralized ledger
- ⚡ Efficient and scalable, supporting infinite random numbers per block
- 🌐 Decentralized and trustless, no need for centralized oracles
- 🔭 Transparent and auditable, with open-source code

## 📚 Overview

RandoMina is a protocol designed to generate pseudorandom numbers on the Mina blockchain. It is implemented as a `SmartContract` in [o1js](https://github.com/o1-labs/o1js), and can be either deployed and used individually, or as a part of a customized solution. By leveraging recursive zkSNARKs, RandoMina ensures that the generated random numbers are provably random, fresh and tamper-proof. These properties make RandoMina suitable for a wide range of applications, such as gaming, lotteries, and decentralized finance (DeFi).

## How It Works 🔍
RandoMina employs a combination of on-chain data and user-specific details to generate pseudorandom numbers:

- **Network State**: Utilizes the current network state, including block attributes and timestamps, to maintain freshness and prevent precomputation or prediction of future values.
- **Sender-Specific Nonce**: Incorporates a user-specific nonce, derived from the sender's public key, to ensure that each participant generates unique random numbers.
- **Local Seed/Nonce**: Allows each user to generate multiple pseudorandom numbers within a single block by varying their own private nonce.

## Integration with zkApps and Smart Contracts 🧩

The RandoMinaContract smart contract facilitates the generation and the verification of pseudorandom numbers by users. It ensures that the numbers are produced using the correct network state and sender information. This contract can be integrated with or serve as a foundation for other smart contracts requiring random number generation. It can be natively used on the Mina blockchain

## 🚀 Getting Started

### 💻 Example Usage

```typescript
// 1. Geneerate random number. This is done off-chain.
const senderPublicKey: PublicKey = getSenderPublicKey(); // set the public key of the sender. this is the account that generates the random number

const networkState: NetworkValue = Mina.activeInstance.getNetworkState();
const currentState: Field = networkState.stakingEpochData.ledger.hash;
const sender: Field = Poseidon.hash(senderPublicKey.toFields());

const randomNumberProof: RandomNumberObservationCircuitProof = await RandomNumberObservationCircuit.generateRandomNumber(
    { networkState: currentState, sender: sender }, // public PRNG params
    randomNonce, // private PRNG param
);

// 1.1 The random number can be extracted from the public output
const randomNumber: Field = randomNumberProof.publicOutput;

// 2. Verify random number. This is done on-chain. Once the proof is verified, the random number can be used in other smart conracts.
// This is achieved by leveraging recursive zkSNARKs.
const txn: Mina.Transaction = await Mina.transaction({ sender: feePayerPublicKey, fee: transactionFee }, () => {
    zkAppInstance.verifyRandomNumber(randomNumberProof);
});
```

### 🛠️ How to build

```sh
npm run build
```

### 🧪 How to run tests

```sh
npm run test
npm run testw # watch mode
```

### 📊 How to run coverage

```sh
npm run coverage
```


---

Made with 💜 by the [zkLocus](https://zklocus.dev/) Team | [Illya Gerasymchuk](https://illya.sh/)