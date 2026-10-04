# Week 4 Studio Results - RSA and Timing Side Channels

## Task 2: Shared-factor scan

`batch_gcd_recover` checks every unordered pair of public moduli. For a pair
`(n_i, n_j)`, `gcd(n_i, n_j)` is `1` when the keys have no factor in common;
when it is a nontrivial value, that value is the shared prime. The shared prime
lets `factor_from_shared` derive `phi(n)` and the private exponent `d` for both
indices in the pair. The provided corpus therefore recovers both vulnerable
keys while leaving isolated keys unrecovered.

The naive scan costs one GCD per pair, or

$$\binom{k}{2} = \frac{k(k-1)}{2} = O(k^2)$$

for `k` public keys. Doubling the corpus approximately quadruples the number of
GCDs. An internet-scale scan is still practical because GCDs are cheap, and a
product tree plus remainder tree reduces the batch computation to near-linear
work in the corpus size, rather than doing every pair explicitly.

## Task 3: Timing attack and fix

The attack builds 256 guesses at each byte position using the recovered prefix.
The correct candidate matches one additional byte in `insecure_equal`, so its
call is slower. `time_guesses` measures all candidates in an interleaved order
for each round and uses a median per candidate. The supplied tests use 41 rounds;
that was sufficient for a stable signal in this environment.

`constant_time_equal` checks the lengths once, then visits every byte and
accumulates all differences without returning early. In production,
`hmac.compare_digest` should be used instead of a hand-written comparison.

## Full scenario explanation

RSA does not become insecure because the RSA arithmetic is incorrect. For a
public key `(n, e)`, recovering the private exponent `d` normally requires
`phi(n)`, and calculating `phi(n)` requires the prime factors `p` and `q` of
`n = p * q`. With properly generated RSA-2048 keys, factoring an isolated
modulus is intended to be infeasible.

The first attack changes the problem from factoring one modulus to comparing a
population of moduli. If two devices accidentally generate a common prime,
then `gcd(n_i, n_j)` returns that prime immediately. Dividing each modulus by
the shared prime reveals the other factor, after which `factor_from_shared`
computes `phi(n)` and `d`. The implementation checks every unordered pair and
stores the recovered private exponent for both indices. In this corpus, keys 0
and 4 share a prime; the six other keys remain unrecovered because their GCDs
with every other modulus are 1.

The second attack targets an implementation condition rather than key
generation. `insecure_equal` returns as soon as a byte differs. A guess with a
correct prefix therefore runs longer than a guess that differs earlier. The
timing attack keeps the already recovered prefix, tries all 256 possibilities
for the next byte, pads each guess to the full secret length, and selects the
slowest median. `time_guesses` interleaves candidates across 41 rounds, which
reduces bias from CPU drift and scheduler noise. The test recovers `a53c`
through timing alone without reading the secret.

The fix accumulates `x ^ y` for every byte and only compares the accumulator at
the end. The same attack then cannot recover the secret because a mismatch no
longer causes an early return. Production code should use
`hmac.compare_digest`; a hand-written comparison must be reviewed for every
input-dependent path, including length handling.

## Task 4: Control Scorecards

The tables use the required `Before`, `After control`, and `Evidence` columns.
The lab measures correctness and attack behavior, but does not provide a
benign-traffic corpus, deployment metrics, or production monitoring. Those
limits are recorded instead of being presented as measurements.

### Control: RSA-2048 key generation and encryption

| Axis | Before | After control | Evidence |
|---|---|---|---|
| Threat model | Attacker knows the public moduli and exponents, but not the private factors. | Unchanged; the attacker can collect and compare many public keys. | `test_rsa.py` supplies eight public keys and tests the population attack. |
| Guarantee | No guarantee if prime generation reuses entropy or produces related primes. | Factoring an isolated `n` remains infeasible **provided** `p` and `q` are strong, secret, and independently generated. | RSA reduction in `test_rsa.py`; the guarantee is conditional on key-generation quality. |
| Coverage | A single-key factoring attack is outside the practical lab budget. | Detects the shared-prime failure for every pair in the supplied corpus. | Pairwise scan; both vulnerable indices `0` and `4` are recovered. |
| Bypass | Reuse a weak RNG state or otherwise cause two keys to share a prime. | **Found:** the shared-factor attack bypasses the intended factoring difficulty. | `gcd(n_0, n_4)` reveals the shared factor and both private exponents. |
| FP cost | No scan result exists before the control. | No false-positive measurement is available; the scan should report only nontrivial shared factors. | The six isolated keys are not returned by `batch_gcd_recover`. |
| Op cost | No cross-key scan. | Naive implementation performs `k(k-1)/2 = O(k^2)` GCDs; product/remainder trees reduce large-scale work toward linear. | Complexity analysis in Task 2; no production benchmark was run. |
| Observability | A reused prime may be invisible when keys are inspected one at a time. | A nontrivial GCD identifies the vulnerable key indices and supports key rotation. | Recovered index output and the `safe_keys_not_recovered` test. |
| Failure mode | Silently issues keys whose individual moduli look normal but are jointly vulnerable. | Fails to provide protection against keys already generated with shared factors; detection must trigger revocation and replacement. | The guarantee test demonstrates both keys fall once the population is scanned. |

### Control: constant-time secret comparison

| Axis | Before | After control | Evidence |
|---|---|---|---|
| Threat model | Attacker can submit guesses and measure oracle response duration, but cannot read the secret. | Same attacker and oracle access. | `make_oracle` exposes only comparison results and timing. |
| Guarantee | `insecure_equal` leaks the matching-prefix length through early-exit timing. | Comparison reveals no useful prefix information through duration **provided every input-dependent path is constant-time**. | The timing attack recovers `a53c` before the fix and fails after it. |
| Coverage | Prefix guesses can be tested byte by byte. | Covers equal-length byte comparisons used by this lab; it is not a proof about unrelated code paths. | `test_timing_attack_recovers_secret` and `test_constant_time_defeats_timing_attack`. |
| Bypass | Guess bytes and use repeated timing measurements. | **Found before control:** a correct prefix produces one more `AMPLIFY` loop and wins the timing comparison. | The 256-candidate scan recovers both secret bytes. |
| FP cost | No benign comparison corpus was supplied. | Functional equality remains correct; no false-positive rate was measured. | `test_constant_time_equal_is_correct` checks equal, unequal, and unequal-length inputs. |
| Op cost | Early exit is cheaper for mismatching inputs. | Every byte is visited, adding work proportional to the compared length; 41 timing rounds are used by the attack test. | `constant_time_equal` loops over all bytes; no deployment benchmark was run. |
| Observability | Timing leakage is normally silent. | The comparison itself emits no alert; timing tests or code review must detect regressions. | The guarantee test is the available regression check. |
| Failure mode | Silently leaks information and can expose the secret one byte at a time. | A reintroduced early return, variable-length path, or other timing difference silently restores the leak. | The implementation uses an accumulator and does not return inside the byte loop. |

## Limitations

The corpus is small and synthetic, and the timing result depends on machine
load and runtime noise. The scan note describes asymptotic cost; it does not
benchmark a product/remainder-tree implementation or claim that all
internet-scale operational costs are negligible.

## Verification

Running `python3 test_rsa.py` reports all six provided tests passing.