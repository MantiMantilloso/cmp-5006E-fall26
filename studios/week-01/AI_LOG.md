### starter.py

**Tool:** Copilot, GPT-5.6 Luna

**What I asked:** "Help me implement the two required functions in the substitution-cipher studio: `frequency_guess_key(ciphertext)` and `crack(ciphertext, restarts, iters, seed)`. The functions should use the public English frequency table and the bigram score to recover the plaintext without the key."

**What I got:** A working implementation strategy and code for both functions. The first function maps the most common ciphertext symbols to the most common English letters by rank, and the second function performs a seeded random-restart hill climb using the public `score` function to improve the guess until it produces English-like text.

**What I did with it:**
  I copied the logic into `starter.py`, fixed the missing `random` import needed for the seeded hill climb, and validated the result by running `python3 starter.py` and then `python3 test_cipher.py` in the studio folder.

**Did I understand it?** Yes. I understand that the frequency-rank step is a noisy first pass and the hill climb is what turns it into a readable or fully recovered plaintext by rewarding English bigram structure. The real lesson is that a substitution cipher is only secure if the plaintext has no exploitable structure; English plaintext violates that assumption.

