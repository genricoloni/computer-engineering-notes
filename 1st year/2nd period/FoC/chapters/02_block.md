# Block ciphers

BLock ciphers are the most used type of encryption. The intuition is simple: we take the entire block of data, and we split it into smaller blocks with fixed length, and we encrypt each block separately. The length is fixed by the chosen algorithm, like the encryption and decryption functions. In general, we can say that:

- $E_k: \{0,1\}^n \rightarrow \{0,1\}^n$, and outputs, in fact, a keyed permutation of the input. It should be **efficient**, **bijective** and **invertible**.
- $D_k: \{0,1\}^n \rightarrow \{0,1\}^n$.

## True random ciphers

Given the fact that, given $n$ the length of the block, there are $2^n!$ possible permutations, a true random cipher should implement all of them: this is impossible, because the size of the key would be $\log_2(2n!) \approx (n-1.44)\cdot2^n$, which is exponential in $n$; given also the fact that the block size cannot be small, in order to **avoid dictionary attacks** (common values of $n$ are 64 or 128), we conclude that a true random cipher is impossible to be implemented.

## Pseudo-random permutation

Considering a family of permutation $k \in K = {0,1}^k$, $E_k : {0,1}^n \rightarrow {0,1}^n$ is a pseudo-random permutation if it's indistinguishable from a true random permutation, given a random key $k$. The number of possible pseudo-random permutations is $2^{k}$, and it's the practical instance of a true random permutation.

### Practical block ciphers

In real life applications, we intend Indistinguishability as the capacity of the encryption function to choose a key in a way that appears to be random to an adversary, that is able to perform a **exaustive search** on the key space.

![Indistinguishability](../images/02/indisti.png){ width=500px}

Let assume that the adversary has access to an encryption oracle, so they cannot look into the box, but only submit an input and get the relative output. The box, which is our algorithm that choose the permutation, is considerate secure if the adversary cannot guess which permutation has bee used with a probability larger that $0.5$. Thus, the output of a block cipher is a uniform random variable, no efficient analytical attack
is possible.

#### Brute force attack - Exhaustive search

This is a known-plaintext attack, where the adversary has access to a set of plaintext-ciphertext pairs. Given one of these pairs, check whether the plaintext is the correct one, by trying all the possible keys. The complexity of this attack is $2^k$, where $k$ is the length of the key.

#### False positive

The attack also have to deal with **false positives**: given a pair of plaintext-ciphertext, such that the ciphertext is the result of the encryption of the plaintext for a given key, we have to determines the number of keys that map the plaintext into the ciphertext. This can be calculated considering that $P[k] = P[E_{k^*}(\text{plaintext}) = \text{ciphertext}] = 2^{-n}$, where $k^*$ is the correct key, so the number of keys that map the plaintext into the ciphertext is $2^k \cdot 2^{-n} = 2^{k-n}$.

