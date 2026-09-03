## Recovered plaintext: 

The attack recovered the ciphertext without the key, producing the original English text before any manual correction. The initial frequency-only pass was noisy but readable; the hill-climbing refinement recovered essentially the whole plaintext.

## Assumption: 

The attack relied on the assumption that the plaintext is English, so letter frequencies and bigram patterns leak through the substitution cipher.

## Defense: 

To defeat the attack, I can encode the message into a non-English or symbol-heavy format so that the frequency distribution and bigram structure no longer resemble English. In Control Scorecard axis 2 terms, this guarantees confidentiality only under the condition that the message remains non-linguistic; the cost is reduced readability, compatibility, and usability.

## Failure Atlas: 

Frequency analysis can fail on short texts because a small sample does not match the long-run English frequency distribution. Rare letters are underrepresented, so the algorithm ranks them incorrectly and produces a misleading but readable-looking mapping.

The substition cipher can easily be broken by following the expected structure of the english language. 