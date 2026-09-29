# Bitcoin Verification Lab

Don't trust. Verify. — Walk through real Bitcoin code, math, and network properties to prove the claims yourself. No coding required.

Enter as a skeptic. Leave as your own bank.

---

## How This Lab Works

1. Open an experiment — Each one tackles a claim about Bitcoin.
2. Play with the demo — Interactive tools show you how the claim works.
3. Follow the verification steps — Simple actions you can take to prove it yourself.
4. Ask an AI — Get a prompt you can paste into ChatGPT, Claude, or any AI to double-check the code and math.
5. Check it off when you're satisfied. Collect all 11 for full verification.

---

## Experiment 1: The 21 Million Supply Cap

**Claim:** "There will only ever be 21 million bitcoin"

This is the most important claim — and the easiest to verify. The limit isn't a promise; it's written directly in Bitcoin's source code, enforced by every single computer running the Bitcoin software.

### The Actual Bitcoin Code

This is real C++ code from Bitcoin Core (src/validation.cpp). Every line is public. Anyone can read it.

```cpp
CAmount GetBlockSubsidy(int nHeight, const Consensus::Params& consensusParams)
{
    // How many halvings have happened so far?
    int halvings = nHeight / 210000;  // 210,000 blocks = ~4 years

    // Safety: if halvings goes over 64, reward = 0
    if (halvings >= 64)
        return 0;

    // Start at 50 BTC per block
    CAmount nSubsidy = 50 * COIN;   // 5,000,000,000 satoshis

    // The magic line: cut the subsidy in half for each halving
    nSubsidy >>= halvings;  // "shift right" = divide by 2 each time

    return nSubsidy;
}
```

**Key Line:** `nSubsidy >>= halvings` means: "Start at 50 BTC, then cut in half every 210,000 blocks"

**The Result:** If you add up all the block rewards after 32 halvings, the total is approximately 20,999,999.9769 BTC. The code implicitly creates a 21M cap through the halving math — the exponential decay mathematically guarantees it can never exceed ~21M.

**Enforcement:** Every node checks every block. If a miner tried to mint more than allowed, the block would be immediately rejected by the entire network.

### How to Verify This Yourself

1. Visit github.com/bitcoin/bitcoin — no account needed.
2. Search for "GetBlockSubsidy" in the search bar (top of GitHub).
3. Read it — you'll see the exact code above. The function controls how much new BTC is created.
4. Do the math: 50 + 25 + 12.5 + 6.25 + 3.125 + ... = ~100. Each "era" is 210k blocks x the reward. Add them all up and the total is 21M.
5. Optional: Download Bitcoin Core from bitcoin.org and run a full node — your computer independently verifies every block and every coin ever created.

### Verify with AI

Paste this into ChatGPT, Claude, or any AI:

```
I want to verify Bitcoin's 21 million supply cap. Here is the GetBlockSubsidy function from Bitcoin Core's source code (C++). Please explain line by line what this code does, how it enforces the supply limit, and confirm whether or not the 21 million cap is mathematically guaranteed. Code:

CAmount GetBlockSubsidy(int nHeight, const Consensus::Params& consensusParams) {
    int halvings = nHeight / 210000;
    if (halvings >= 64) return 0;
    CAmount nSubsidy = 50 * COIN;
    nSubsidy >>= halvings;
    return nSubsidy;
}

Is it true that there will never be more than ~21 million bitcoin, and where specifically in the code is that enforced? Please be thorough and honest about any limitations.
```

Links: [View on GitHub](https://github.com/bitcoin/bitcoin/blob/master/src/validation.cpp) | [WolframAlpha math](https://www.wolframalpha.com/input?i=Sum%5B210000+*+%2850%2F2%5Ei%29%2C+%7Bi%2C0%2C32%7D%5D)

---

## Experiment 2: Immutability — The Block Chain

**Claim:** "Once a transaction is confirmed, it cannot be changed or reversed"

Each block links to the previous one using a cryptographic fingerprint called a hash. If you change anything in a block — even one character — its hash changes, breaking the chain. This makes tampering instantly detectable.

### How It Works

Each block header contains `hashPrevBlock` — the hash of the previous block. This creates an unbreakable chain: you'd have to re-mine every single block from the point of change onward, and do it faster than the entire Bitcoin network. That's computationally impossible after just a few confirmations.

**Avalanche Effect:** Changing one bit in a block produces a completely different hash. There's no way to predict or control what the new hash will be.

### How to Verify This Yourself

1. Use a block explorer like mempool.space.
2. Click any block and find the "Previous Block Hash" field.
3. Copy the previous hash and search for it — it will match the actual previous block's hash.
4. Try this for 5 blocks in a row — every single one chains to the previous. There's no gap, no break.
5. Deep verification: Run a full node. Your node independently validates every block and every hash from the genesis block in 2009 to today.

### Verify with AI

```
Explain how Bitcoin's blockchain achieves immutability. I want to understand:
1. What is a cryptographic hash and why is it called a "fingerprint"?
2. How does each block reference the previous block's hash?
3. What happens if someone tries to modify a transaction in an old block?
4. Why does it get exponentially harder to change older blocks?
5. Is it true that after 6 confirmations (about 1 hour) a Bitcoin transaction is practically irreversible?

Please explain this in simple terms that someone with no technical background can understand.
```

Link: [Merkle tree code on GitHub](https://github.com/bitcoin/bitcoin/blob/master/src/consensus/merkle.cpp)

---

## Experiment 3: Decentralized Network — No Central Server

**Claim:** "Bitcoin has no central server, company, or government controlling it"

Unlike Facebook, Google, or your bank, Bitcoin has no CEO, no headquarters, no central database, and no off switch. Thousands of independent computers (nodes) around the world run the same software and check each other.

### How It Works

Bitcoin uses a peer-to-peer network. When you send bitcoin, your transaction broadcasts to every node. Each node independently validates it against the same rules. There's no central server that says "yes" or "no."

There is no official registry of Bitcoin nodes — they're anonymous and don't self-report. Projects like bitnodes.io estimate tens of thousands of reachable nodes by scanning the network. The true count — including hidden nodes — is unknown, by design. There's no list to ban, no registry to shut down.

**The "Attack" Test:** If any government tried to shut Bitcoin down, they'd need to find and shut down thousands of independently operated nodes in 100+ countries — many running on Tor, in data centers, basements, and laptops. There's no single building to raid.

**Fun fact:** You can run a Bitcoin node on a Raspberry Pi ($35 computer) from your living room. If you do, you're a global peer.

### How to Verify This Yourself

1. Check the live node count at bitnodes.io — a community-powered live count of reachable nodes worldwide.
2. See the geographic spread — nodes exist on every continent except Antarctica.
3. Run your own node: Download Bitcoin Core from bitcoin.org and run it. You're now an equal peer on the network.
4. Test censorship: Use a Tor-enabled wallet to send a transaction. It goes through exactly the same as any other.

### Verify with AI

```
Explain how Bitcoin is decentralized. I want to understand:
1. Is there a central server or company that runs Bitcoin?
2. How do Bitcoin nodes work and how many are there?
3. Can any government shut down Bitcoin? What would it take?
4. How do nodes agree on the rules (consensus)?
5. Can I run a node myself? What equipment do I need?
6. What stops someone from creating a fake Bitcoin and tricking people?

Explain like I'm 15 years old. Be honest about any centralizing forces (mining pools, developers, etc.) — I want the good and the bad.
```

Links: [Live Node Map (Bitnodes)](https://bitnodes.io/) | [Network code on GitHub](https://github.com/bitcoin/bitcoin/blob/master/src/net_processing.cpp)

---

## Experiment 4: Difficulty Adjustment — Self-Balancing

**Claim:** "Bitcoin automatically adjusts mining difficulty to keep blocks at 10 minutes"

Every 2,016 blocks (~2 weeks), every node checks: "Were the last 2,016 blocks found faster or slower than 10 minutes each?" If faster, mining gets harder. If slower, it gets easier. The network self-balances automatically.

### Formula

New Difficulty = Old Difficulty x (Actual Time / Expected Time)

Expected Time = 2,016 blocks x 10 min = 20,160 min (2 weeks)

### The Code

In src/pow.cpp, the `GetNextWorkRequired()` function calculates the new difficulty. The maximum adjustment per cycle is ±400% (4x easier or harder) to prevent wild swings. The code is ~60 lines — any programmer can audit it.

### Why It Matters

Without difficulty adjustment, miners would find blocks in seconds when price is high (instability) or take days when price is low (transactions never confirm). The adjustment keeps Bitcoin running smoothly whether 10 miners or 10 million.

### How to Verify This Yourself

1. Check the last adjustment at mempool.space/graphs/mining/difficulty.
2. Look at the history — scroll back years. Difficulty has gone up and down as miners joined and left. The network never broke.
3. Watch the countdown — the next adjustment is always visible, every 2,016 blocks.
4. Code verification: The adjustment code is at github.com/bitcoin/bitcoin/src/pow.cpp. You can read every line.

### Verify with AI

```
Explain Bitcoin's difficulty adjustment mechanism to me in plain language. I want to understand:
1. How often does difficulty adjust and why 2,016 blocks?
2. What is the formula? New Difficulty = Old Difficulty x (Actual Time / Expected Time)
3. What prevents the difficulty from swinging wildly?
4. Does this mean Bitcoin's block time is always 10 minutes?
5. Where is this in the actual Bitcoin source code?

Give concrete examples: what happens if 10x more miners join? What if half the miners leave?
```

Links: [pow.cpp on GitHub](https://github.com/bitcoin/bitcoin/blob/master/src/pow.cpp) | [Live difficulty chart](https://mempool.space/graphs/mining/difficulty)

---

## Experiment 5: Seed Phrase Security — Impossible to Guess

**Claim:** "Your 12-word seed phrase cannot be brute-forced, even by all computers in the universe"

A 12-word BIP39 seed phrase uses 2048 possible words chosen at random. That gives 2048^12 combinations — a number so astronomically large it defeats every computer that could ever exist.

### The Numbers

- 12-word seed combinations (2048^12): 5.4 x 10^39
- Grains of sand on Earth: 7.5 x 10^18
- Atoms on Earth: 10^50

If you checked 1 billion seeds per second since the Big Bang (13.8B years ago), you'd have checked 0.0000000001% of all possible 12-word seeds.

A 24-word seed (2^256) has more combinations than there are atoms in the observable universe — by many orders of magnitude.

### The REAL Danger

The math is unbreakable — but humans break their own security by: taking photos of their seed, typing it online, using phishing sites, or generating seeds with bad random number generators. The system is secure. The user is the weakest link.

### How to Verify This Yourself

1. See the word list at github.com/bitcoin/bips/bip-0039/english.txt — 2,048 words.
2. Do the math: 2048^12 = 5.4 x 10^39. Type "2048^12" into Google.
3. Understand the scale: There are "only" 10^80 atoms in the observable universe. 2048^24 is larger than 10^80.
4. Read the BIP39 specification at github.com/bitcoin/bips/blob/master/bip-0039.mediawiki.

### Verify with AI

```
I'm trying to understand Bitcoin seed phrase security. Please verify and explain:
1. A 12-word BIP39 seed phrase uses 2048 possible words. How many combinations is 2048^12?
2. Write out the full number: 2048^12 = ?
3. Compare this number to: grains of sand on Earth, atoms in the human body, age of the universe in seconds, and atoms in the observable universe.
4. If every computer on Earth worked together to brute-force a single 12-word seed, how long would it take?
5. What are the actual risks to seed phrases (not the math, but human behavior)?

Be honest — is the math actually unbreakable? What about quantum computing?
```

Links: [BIP39 Specification](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) | [Word List (2048 words)](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt)

---

## Experiment 6: The Halving Schedule — Predictable Supply

**Claim:** "New bitcoin creation is cut in half every 4 years, predictably, until ~2140"

Every 210,000 blocks (~4 years), the reward for mining a block is cut in half. This is hard-coded, not voted on. It will happen whether you want it to or not, until the subsidy reaches zero around 2140.

### Halving Timeline

- 2009 — Era 0: 50 BTC
- 2012 — Era 1: 25 BTC
- 2016 — Era 2: 12.5 BTC
- 2020 — Era 3: 6.25 BTC
- 2024 — Era 4: 3.125 BTC ← YOU ARE HERE
- 2028 — Era 5: 1.563 BTC
- 2032 — Era 6: 0.781 BTC
- ...→ 2140: ~0 BTC

The halving interval of 210,000 blocks is set in chainparams.cpp:
```cpp
consensusParams.nSubsidyHalvingInterval = 210000; // Every ~4 years
```

After 32 halvings (33 eras including the first), the reward becomes so small it rounds to zero. This is when mining will be entirely supported by transaction fees.

### How to Verify This Yourself

1. Check the halving countdown at mempool.space — it shows the block height and when the next halving is expected.
2. Look at history — Bitcoin has had exactly 4 halvings (2012, 2016, 2020, 2024). Each one cut the reward exactly in half.
3. Read the code: `consensusParams.nSubsidyHalvingInterval = 210000` is in the public Bitcoin Core source code.
4. Run your own node — it enforces this rule independently.

### Verify with AI

```
Explain Bitcoin's halving schedule to me. I want to verify:
1. How many blocks between halvings and why 210,000?
2. List all the halvings that have happened so far with dates and block rewards.
3. When is the next halving expected? How many will there be total?
4. How does the halving schedule guarantee the 21 million limit?
5. What happens after all bitcoin are mined? Will the network stop working?
6. Can the halving schedule be changed? What would it take?
```

---

## Experiment 7: Open Source — Anyone Can Read Every Line

**Claim:** "Bitcoin's code is completely open source. Anyone can review it for backdoors or hidden features"

Bitcoin's code isn't secret — it's open source under the MIT license. Bitcoin Core has been reviewed by thousands of developers, security researchers, and academics over 16+ years. There are no hidden backdoors, no secret inflation, no "hidden mint" button.

- 16+ years of review
- 1,000+ contributors
- ~800K lines of code
- 30,000+ GitHub stars

**What "Open Source" Means:** The MIT license says anyone can copy, modify, and distribute the software. There is no company behind it. If someone added a backdoor, every node operator would see the change and reject it.

**Bitcoin is not Bitcoin Core:** There are multiple independent implementations of the Bitcoin protocol: Bitcoin Core (C++), btcd (Go), and others. They all enforce the same consensus rules. If one had a bug, the others would reject it.

### How to Verify This Yourself

1. Go to github.com/bitcoin/bitcoin — no account needed to browse.
2. Browse the code: click into src/, then consensus/, then validation.cpp.
3. Check the commit history — see every change ever made to Bitcoin Core.
4. Look at the issues — see real bug reports and discussions.
5. Check for alternative clients — search for "btcd," an independent full node implementation in Go.

### Verify with AI

```
I want to verify that Bitcoin is truly open source and that there are no hidden backdoors. Please help me understand:
1. What open source license does Bitcoin Core use?
2. How many years has it been publicly reviewed?
3. Could someone add a backdoor without anyone noticing?
4. Are there multiple independent implementations of Bitcoin?
5. What would happen if a developer tried to add code that mints extra coins?
6. Can I download, compile, and run Bitcoin Core myself to verify it's the same code?

Include links to the actual GitHub repository and any independent implementations.
```

Links: [Bitcoin Core on GitHub](https://github.com/bitcoin/bitcoin) | [btcd (Go implementation)](https://github.com/btcsuite/btcd)

---

## Experiment 8: Censorship Resistance — No One Can Freeze You

**Claim:** "No government, bank, or company can block or reverse a Bitcoin transaction"

This is the superpower that makes Bitcoin different from every digital payment system before it. Once a transaction is confirmed, no one on Earth can reverse it. No CEO, no court order, no government decree. The math decides, not people.

### What Bitcoin CAN Do
- Send money to anyone, anywhere, anytime
- Receive money without asking permission
- Hold your own keys (be your own bank)
- Send as little as 0.00000001 BTC (~$0.001)
- Transact on a Sunday, Christmas, or during a bank holiday

### What Bitcoin CANNOT Do (This Is Good)
- Freeze your account
- Reverse a confirmed transaction
- Block a valid transaction from being mined
- Create new coins from nothing
- Charge you fees without your consent

### How It Works

Every node validates every transaction independently. A transaction that pays the right fees, has valid signatures, and spends unspent outputs will be accepted by the network regardless of who you are. The nodes don't know your name, your nationality, or your politics — they only check math.

### How to Verify This Yourself

1. Send a transaction using any wallet. It goes through — no approval needed.
2. Check a block explorer like mempool.space — search for a recent transaction. It's there for everyone to see.
3. Run a node — you decide which transactions to accept. No one can force an invalid transaction on your node.
4. Read about real examples: WikiLeaks (2010), the Canadian trucker protests (2022).

### Verify with AI

```
I want to understand Bitcoin's censorship resistance. Please explain:
1. What makes Bitcoin transactions uncensorable? Is it absolute or are there limits?
2. How do nodes prevent anyone from blocking a valid transaction?
3. What is a 51% attack and what can/can't an attacker do?
4. Give real historical examples where Bitcoin resisted censorship attempts.
5. How does Bitcoin compare to traditional banking or PayPal in terms of censorship?
6. Are there any realistic threats to Bitcoin's censorship resistance in the future?

Be balanced — mention the real risks but also explain why Bitcoin remains the most censorship-resistant payment system ever created.
```

Link: [Transaction validation code](https://github.com/bitcoin/bitcoin/blob/master/src/validation.cpp)

---

## Experiment 9: Bitcoin as Sound Money

**Claim:** "Bitcoin is better money than gold, fiat, or any alternative humanity has tried"

### The Money Scorecard

| Property | Bitcoin | Gold | Fiat |
|---|---|---|---|
| 1. Scarce | Perfect | Good | Poor |
| 2. Durable | Perfect | Perfect | Poor |
| 3. Portable | Perfect | Poor | Okay |
| 4. Divisible | Perfect | Poor | Okay |
| 5. Fungible | Good | Perfect | Okay |
| 6. Recognizable | Good | Okay | Perfect |
| 7. Store of Value | Perfect | Good | Poor |

Bitcoin scores 6/7 perfect, 1/7 good. Gold scores 2/7 perfect. Fiat scores 1/7 perfect. Bitcoin is the first money in history that scores "good" or better on every single property.

### The History of Money

- 10,000 BC: Barter / Cowrie Shells
- 600 BC: Gold & Silver Coins — the gold standard prevailed for ~2,600 years
- 1600s: Paper Money (Gold-Backed) — receipts for gold, then governments detached from gold
- 1971: Pure Fiat — "Nothing backs it but government decree." Since then: 87% dollar devaluation
- 2009: Bitcoin — First money with mathematically provable scarcity. No issuer. No borders. No inflation.

### How to Verify This Yourself

1. Define "good money" for yourself before reading any economics. Then check against the 7 properties.
2. Every fiat currency in history has eventually gone to zero. Research "fiat currency lifespan."
3. Test portability: try sending $10 via bank wire internationally vs. sending $10 in Bitcoin.
4. Check the M2 money supply chart vs. Bitcoin's supply at mempool.space.
5. Read the Bitcoin whitepaper at bitcoin.org/bitcoin.pdf — 9 pages.

### Verify with AI

```
I'm trying to understand what makes "good money" and whether Bitcoin qualifies. Please help me think through this:
1. What are the generally accepted properties of good money?
2. How does Bitcoin score on each property compared to gold and fiat?
3. What are the weaknesses of each: Bitcoin, gold, and fiat?
4. Every fiat currency in history has eventually failed. Is that true?
5. If Bitcoin has superior properties to both gold and fiat on paper, why hasn't it replaced them yet?
6. Is "money" something governments decide or something that emerges naturally from the market?

Please be intellectually honest — give me the real arguments for AND against each form of money.
```

Links: [Bitcoin Whitepaper](https://bitcoin.org/bitcoin.pdf) | [US M2 Money Supply](https://fred.stlouisfed.org/series/M2SL) | [Live Bitcoin Supply](https://mempool.space/)

---

## Experiment 10: Consensus & Policy Rules

**Claim:** "No single person, company, or government can change Bitcoin's rules. Changes require near-universal agreement"

*(Full content continues with consensus rules, policy rules, UASF, hard forks vs soft forks, and verification steps. See the live page for the complete interactive experience.)*

---

## Experiment 11: Final Challenge

**Claim:** "You can verify every claim about Bitcoin yourself. You don't need to trust anyone."

*(The final experiment ties all 10 together with a comprehensive verification challenge.)*

---

Created by Casey Karlow. Live data from mempool.space. Source code references from Bitcoin Core (MIT license).
