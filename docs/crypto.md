# YeeChain Crypto 

 Cryptography is the fundamental building block of everything in the blockchain with the most extreme importance. It deserves a thorough and careful study.
 
 At the early stage of the development of YeeChain Poc/Test project, to make the things simple and fast, we tend to borrow most of the ideas from Bitcoin and Ethereum
 as they provide the basic, time-proven solutions. 
 
 ## Cryptographic Algorithm
 The class of Elliptic curve algorithm will be chosen undoubtedly.
 
 ## Key&Keystore
 An ECDSA private/public key pair serves as a user’s account.
 
 Keystore provide different methods to save the keys. Before saved, keys has been encrypted in keystore.
 
 ## Hash
 Support generic hash functions, such as sha256, sha3-256, ripemd160 etc.

 ## Signature & Verify
 The cryptographic signatures used in Bitcoin are ECDSA signatures and use the curve secp256k1. We will do the same.
 
 
 
  
 #
 After most pieces of YeeChain's core modules has the preliminary prototype ready and can run together, some advance topics of Crypto then need to be considered and integrated into the system as needed. More detail decision should be made then.
 
 ## zkSNARKs 
 [Zero-Knowledge Proof](https://en.wikipedia.org/wiki/Zero-knowledge_proof) is an relatively old cryptography concept. It's interesting but not well known by ordinary people outside the cryptographic circles compare to other idea such as public key cryptosystem etc. 
 But the concept has show it's amazing potential in the blockchain world.
 
 zkSNARKs is the abbreviation of zero-knowledge succinct non-interactive arguments of knowledge.
 The possibilities of zkSNARKs are impressive, you can verify the correctness of computations without having to execute them and you will not even learn what was executed – just that it was done correctly
 
 ## Privacy Homomorphism
 Privacy Homomorphism, or [Homomorphic encryption](https://en.wikipedia.org/wiki/Homomorphic_encryption),  is a form of encryption that allows computation on ciphertexts, generating an encrypted result which, when decrypted, matches the result of the operations as if they had been performed on the plaintext. The purpose of homomorphic encryption is to allow computation on encrypted data.
 Fully homomorphic encryption(PHE) is powerful but still at the stage of theoretical research. But partially homomorphic is ready for specific tasks.

 
 ## Schnorr
 [Schnorr signature](https://en.wikipedia.org/wiki/Schnorr_signature) is efficient and generates short signatures for multi signatures.
 
 ## Quantum Computations
 A quantum computer would be able to factor a large number far more quickly than the best available methods today. 
 Elliptic curve cryptography is also vulnerable to a modified [Shor's algorithm](https://en.wikipedia.org/wiki/Shor%27s_algorithm) for solving the discrete logarithm problem on elliptic curves.
 Quantum computation can have potential to subvert the current cryptographic foundation.
 Although, this is still hypothetical，A realistic such machine has not been invented now and we guess will not in the near future.
 But we be better keep an eye on it seriously.
 Actually, we have initiate a move on this topic partner with some of top academic research institutions.
 
 ## Multinational Standard of Cryptography
 We will discuss this later...
 
 
 ## Others...
