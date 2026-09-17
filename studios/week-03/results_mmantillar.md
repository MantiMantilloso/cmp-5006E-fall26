# Week 3 Studio Results - Modes and Misuse

## How the implementations work

### Task 1: ECB structure leak

`ecb_leak_count(image, key)` passes the image and key to `ecb_encrypt`, then
passes the resulting ciphertext to `distinct_blocks`. ECB encrypts each block
independently with the same key, so equal plaintext blocks produce equal
ciphertext blocks. Counting distinct ciphertext blocks therefore reveals the
number of distinct plaintext blocks. The supplied image produces `2` under ECB;
CBC produces `96` because each block depends on the previous ciphertext block.

### Task 2: CTR nonce reuse

CTR encryption has the form `ciphertext = plaintext XOR keystream`. Reusing the
same key and nonce gives both messages the same keystream:

```text
c1 XOR c2 = (m1 XOR K) XOR (m2 XOR K) = m1 XOR m2
```

The implementation computes `c1 XOR c2 XOR known_m1`, which cancels `m1` and
leaves `m2`. The result is limited to the length of the known plaintext by the
provided `xor` helper.

### Task 3: Length extension

The vulnerable MAC is `H(secret || message)`. The toy Merkle-Damgard hash exposes
its complete internal state as the tag, so the attacker can continue hashing
without knowing the secret. The implementation calculates the length of the
unknown secret plus the observed message, creates the hash's 4-byte glue
padding, appends the attacker-controlled extension, and resumes `md_hash` with
the observed tag as its IV. The server consequently computes the same tag for
the forged message.

HMAC rejects the analogous keyless forgery because its nested construction does
not expose a resumable state for the message in this way.

## Task 4: Control Scorecard

| Construction | Guarantee (axis 2) | Condition / failure mode |
|---|---|---|
| ECB mode | Confidentiality of individual blocks against an observer who cannot invert the block cipher. | Identical plaintext blocks produce identical ciphertext blocks, so ECB leaks repeated structure and block equality. |
| CBC / CTR modes | Confidentiality of the message when the block cipher is secure and the mode's IV/nonce condition is met. | CBC needs correct IV handling and provides no integrity by itself. CTR requires a unique nonce under each key; reuse reuses the keystream and creates a two-time pad: `c1 XOR c2 = m1 XOR m2`. |
| `H(secret\|\|msg)` MAC | Appears to authenticate messages to a verifier holding the secret. | Merkle-Damgard length extension lets an attacker who knows the tag and secret length forge a valid tag for an extended message without the secret. This is a construction failure, not a broken hash. |
| HMAC | Message authentication without this length-extension attack. | Requires a secret key shared with the verifier; its nested construction prevents resuming the message hash from the exposed tag. |

All four failures demonstrated in this studio are mode or construction misuse,
not breaks of the underlying block cipher or hash primitive.

## Where we may have been unfair, and what we did not test

The tests use a toy block cipher and toy Merkle-Damgard hash, fixed short
messages, and a known plaintext or known secret length where the attack requires
it. They demonstrate the relevant algebra and construction weakness; they do
not measure the practical difficulty of discovering plaintexts, preventing nonce
collisions in a production system, or implementing a complete authenticated
cipher protocol. CBC confidentiality is also not a substitute for authenticated
encryption.

## Verification

Running `python3 test_modes.py` reports all four tests passing:

```text
ok  ECB leaks 2 blocks, CBC hides (96) - the MODE decided, not the cipher
ok  CTR nonce reuse recovered m2 = week-2 two-time pad on a modern mode
ok  H(secret||msg) guarantee COLLAPSED - valid tag forged without the key
ok  HMAC rejected the forgery - better construction, not a better hash

all 4 tests pass
```
