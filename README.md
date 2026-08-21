# Hash Intelligence & Lookup Resources

A curated reference for **hash lookup, password-hash identification, structured-hash analysis, breach checking, and forensic hash intelligence**.

> **Scope:** This README intentionally excludes Telegram bots/groups and stolen-log/credential-search resources. It focuses on legitimate hash-analysis and defensive security tooling.

---

## Contents

- [1. Hash Lookup / Plaintext Database Services](#1-hash-lookup--plaintext-database-services)
- [2. Hash Identifier / Type Detection](#2-hash-identifier--type-detection)
- [3. Salted vs Unsalted Analysis](#3-salted-vs-unsalted-analysis)
- [4. Breach / Exposure Checking](#4-breach--exposure-checking)
- [5. Forensic / Known-File Hash Intelligence](#5-forensic--known-file-hash-intelligence)
- [6. Local / Offline Heavyweight Tools](#6-local--offline-heavyweight-tools)
- [7. Recommended Workflow](#7-recommended-workflow)
- [8. Database-Size Claims: Important Caveat](#8-database-size-claims-important-caveat)
- [9. Privacy & Safety](#9-privacy--safety)

---

# 1. Hash Lookup / Plaintext Database Services

These services are primarily useful when you already have a hash and want to determine whether a previously recovered/plaintext value exists in a precomputed corpus.

| Rank | Service | Main formats | Advertised / documented scale | Notes |
|---|---|---|---:|---|
| 🥇 | **BinSec HashLookup** | MD5, SHA1, NTLM | **7.5B passwords** | Original reference service; large legacy corpus. |
| 🥈 | **CMD5** | MD5, SHA1, SHA256 and others | **~20.408T claimed unique hashes** | Extremely large published claim; some queries may enter a background job. Treat the number as a claim, not an independently audited count. |
| 🥉 | **BaconHash** | MD5, NTLM, SHA1 | **126B+ claimed per algorithm** | One of the strongest currently advertised multi-algorithm corpora. |
| 4 | **CrackCrypt** | MD5, SHA1, NTLM, SHA256, SHA512 | **30B claimed per algorithm / 150B total** | Large multi-algorithm lookup service with API documentation. |
| 5 | **Weakpass Lookup** | MD5, NTLM, SHA1, SHA256 | **26B passwords** | Lookup page says processing can happen client-side. |
| 6 | **NTLM.PW** | NT/NTLM, LM, SHA256 | **8.726B unique hashes** | Particularly relevant for NTLM; bulk/API functionality available. |
| 7 | **CrackStation** | LM, NTLM, MD5, SHA1, SHA224/256/384/512, RIPEMD160, Whirlpool | **15B MD5/SHA1 + 1.5B other** | Long-running classic lookup corpus. |
| 8 | **Hashes.com** | MD5, SHA1, NTLM, SHA256, SHA512, bcrypt, MySQL, WordPress, etc. | Large accumulated database | Broadest all-round lookup + identifier combination in this list. |
| 9 | **MD5Decrypt** | Primarily MD5 plus additional hashes | Large historical corpus | Especially useful as another MD5 source; exact current database scale is not independently established here. |
| 10 | **WhatsMyIP Hash Lookup** | MD5, SHA1 | **~2.23B hashes** | Smaller independent corpus; useful as another cross-check. |

### 🔗 Direct links

- **BinSec:** https://binsec.tools/lookup/hash/
- **CMD5:** https://www.cmd5.org/
- **BaconHash:** https://baconhash.pw/
- **CrackCrypt:** https://crackcrypt.com/
- **Weakpass:** https://weakpass.com/tools/lookup
- **NTLM.PW:** https://ntlm.pw/
- **CrackStation:** https://crackstation.net/
- **Hashes.com:** https://hashes.com/en/decrypt/hash
- **MD5Decrypt:** https://md5decrypt.net/en/
- **WhatsMyIP:** https://www.whatsmyip.org/hash-lookup/

> **Important:** Database-size figures are not directly comparable. One service may count password candidates, another unique hashes, another rows across multiple algorithms, and another several source datasets combined.

---

# 2. Hash Identifier / Type Detection

These tools are useful when the algorithm is unknown and you need to determine likely formats from length, characters, prefixes, delimiters, or known structured syntax.

| Rank | Tool | Strength | Privacy / processing |
|---|---|---|---|
| 🥇 | **Hashes.com Hash Identifier** | Broad format coverage, multiple possibilities, `hash[:salt]`, expert mode | Web service |
| 🥈 | **Toolsana Hash Identifier** | 100+ formats; bcrypt, Argon2, scrypt, PBKDF2, NTLM, MySQL, LDAP, Unix crypt, etc. | Browser tool |
| 🥉 | **J-Kit Hash Identifier** | Strong structured-format analysis; can expose cost/parameters where encoded | Browser tool |
| 4 | **dCode Hash Identifier** | 350+ possible formats / broad algorithm reference | Web service |
| 5 | **BrowserUtils Hash Identifier** | MD5/SHA/bcrypt/Argon2/scrypt/NTLM and local browser processing | Client-side |
| 6 | **Aback Tools** | Confidence-ranked candidate identification | Browser/local processing |
| 7 | **HackUtils Hash Identifier** | Identification plus Hashcat modes | Web tool |
| 8 | **HackerDNA Hash Identifier** | Good ambiguity/context explanations | Web tool |
| 9 | **Toolali Hash Identifier** | 50+ formats and multiple candidates | Web tool |
| 10 | **WebToolMatrix Hash Identifier** | Structured formats and potential salt/parameter analysis | Web tool |
| 11 | **ScanSuite Hash Identifier** | Ranked candidates and confidence analysis | Browser tool |
| 12 | **Gizza Tools** | bcrypt, Argon2, MD5, SHA, NTLM, sha512crypt, PHPass and others | Client-side claims |
| 13 | **KitLab Hash Analyzer** | Digest-family analysis and security-oriented metadata | Browser tool |
| 14 | **Dozenkit Hash Identifier** | Identifier + verifier + reference material | Browser tool |
| 15 | **ZeroServer Hash Identifier** | Lightweight local identification | Browser tool |
| 16 | **mlab.sh Hash Identifier** | Candidate ranking and ambiguity warnings | Client-side |
| 17 | **OneDev Hash Identifier** | Hash format plus common password-hash structures | Web tool |
| 18 | **J-Kit / related local hash tools** | Good for structured crypt formats | Local/browser |
| 19 | **NameThatHash** | Local/offline hash identification | Local |
| 20 | **HashID** | Classic offline hash identification | Local |

### 🔗 Direct links

- **Hashes.com Identifier:** https://hashes.com/en/tools/hash_identifier
- **Toolsana:** https://toolsana.com/tools/hash-identifier/
- **J-Kit:** https://jkit.tools/en/tools/hash-identifier
- **dCode:** https://www.dcode.fr/hash-identifier
- **BrowserUtils:** https://www.browserutils.dev/tools/hash-identifier/
- **Aback Tools:** https://abacktools.com/tools/crypto/utilities/password-hash-identifier
- **HackUtils:** https://hackutils.com/hash-identifier
- **HackerDNA:** https://hackerdna.com/tools/hash-identifier
- **Toolali:** https://toolali.com/cryptography/hash-identifier/
- **WebToolMatrix:** https://webtoolmatrix.com/hash/free-online-identify-hash-type/
- **ScanSuite:** https://scansuite.io/tools/hash-identifier/
- **Gizza:** https://gizza.ai/tools/
- **KitLab:** https://kitlab.app/en/tools/hash-analyzer
- **Dozenkit:** https://dozenkit.com/tools/hash-identifier
- **ZeroServer:** https://zeroserver.tools/hash-identifier/
- **mlab.sh:** https://mlab.sh/tool/hash-identifier

---

# 3. Salted vs Unsalted Analysis

## The critical rule

A raw hash string often **cannot tell you whether the original password hashing process used a salt**.

For example, this is only a 32-character hexadecimal digest:

```text
5f4dcc3b5aa765d61d8327deb882cf99
```

Depending on context, it may be:

```text
MD5
NTLM
MD4
another 128-bit hexadecimal digest
```

The string itself does **not** prove:

```text
salted MD5
unsalted MD5
salted NTLM
unsalted NTLM
```

A better identifier reports **candidate algorithms + confidence** rather than pretending certainty.

## Structured formats are different

### bcrypt

```text
$2b$12$N9qo8uLOickgx2ZMRZoMye...
```

The bcrypt string encodes its cost and salt structure.

### Argon2

```text
$argon2id$v=19$m=65536,t=3,p=4$<salt>$<hash>
```

The PHC-style representation explicitly contains the algorithm parameters and salt.

### PBKDF2 / crypt / application-specific formats

These may expose iterations, salts, peppers, or other parameters depending on the exact serialization used.

### Practical conclusion

```text
Raw digest
   ↓
length / character-set analysis
   ↓
candidate algorithms
   ↓
structured-format parsing
   ↓
extract salt / cost / iterations when encoded
   ↓
context check (application/database/source)
```

---

# 4. Breach / Exposure Checking

These are **not plaintext hash-recovery services**. They answer a different question: whether a password or account identifier has appeared in known breach data.

## Have I Been Pwned – Pwned Passwords

https://haveibeenpwned.com/Passwords

Uses range queries so the complete password hash does not need to be disclosed to the service. Useful for defensive password auditing.

## Have I Been Pwned – Email

https://haveibeenpwned.com/

Checks whether an email address appears in known breach collections.

## GWDG Password Check

https://pwcheck.gwdg.de/

Provides privacy-preserving password exposure checks using range-query mechanisms.

## Google Password Manager Password Checkup

https://passwords.google.com/

Useful for checking saved credentials for compromise, reuse, and weakness.

---

# 5. Forensic / Known-File Hash Intelligence

These should **not** be confused with password plaintext databases.

## CIRCL Hashlookup

https://hashlookup.circl.lu/

Open-source hash intelligence infrastructure oriented toward known files/artifacts and datasets such as NSRL.

GitHub: https://github.com/adulau/hashlookup-server

## HashScanner

https://www.hashscanner.com/

Currently advertises **1.5B+ NIST NSRL file records**, automatic hash-type detection, and bulk lookup capabilities.

Use case:

```text
file hash
   ↓
known artifact / known software / known file identification
```

rather than:

```text
password hash
   ↓
plaintext password
```

---

# 6. Local / Offline Heavyweight Tools

## mdxfind

https://github.com/Cynosureprime/mdxfind

One of the most interesting options when you want to stop depending on public web services.

The project documentation describes support for hundreds of hash constructions, including:

- MD4 / MD5 / NTLM
- SHA1 / SHA256 / SHA512
- salted variants
- bcrypt
- scrypt
- PBKDF2
- crypt formats
- HMAC constructions
- NetNTLMv1/v2
- chained/composed hashes

The project has also documented high-speed lookup/testing against billion-scale local hash collections.

## NameThatHash

Useful for offline hash-format identification when you do not want to submit hashes to a website.

## HashID

Classic local hash identifier with a large signature database and CLI workflow.

## Hashcat

https://hashcat.net/hashcat/

Not a lookup database. It is a local password-recovery/auditing engine supporting many hash modes and CPU/GPU acceleration.

For authorized auditing, it is the natural next step when a hash is **not** present in a public precomputed database.

---

# 7. Recommended Workflow

## A. Unknown hash

```text
                    UNKNOWN HASH
                         │
                         ▼
                Hash identifier
         ┌───────────────┴───────────────┐
         ▼                               ▼
   Hashes.com /                    Toolsana / J-Kit
   dCode / BrowserUtils             second opinion
         │                               │
         └──────────────┬────────────────┘
                        ▼
               Compare candidates
                        │
                        ▼
          Is it a structured format?
               ┌────────┴────────┐
              YES                NO
               │                  │
               ▼                  ▼
       Parse salt/cost/etc.    Use context
                               + confidence
```

## B. Known MD5 / SHA1 / NTLM hash

For authorized defensive checking:

```text
Hash
 │
 ├── BinSec
 ├── BaconHash
 ├── Weakpass
 ├── CrackCrypt
 ├── Hashes.com
 ├── CrackStation
 └── NTLM.PW (for NTLM)
```

Compare independent results rather than assuming a single database is complete.

## C. No public lookup hit

```text
public databases
      ↓
no result
      ↓
identify exact format
      ↓
check whether salt / iterations / cost are known
      ↓
authorized local auditing
      ↓
mdxfind / Hashcat / controlled wordlists
```

## D. Password-manager audit

Prefer:

```text
local hash identification
        ↓
HIBP / k-anonymity breach check
        ↓
local password policy checks
        ↓
only then consider authorized offline analysis
```

Do not upload actual live passwords to arbitrary lookup websites.

---

# 8. Database-Size Claims: Important Caveat

Huge numbers look impressive, but they are **not automatically comparable**.

Examples:

```text
7.5B passwords
26B passwords
126B entries per algorithm
30B lines per algorithm
20.408T unique hashes claimed
```

These may represent completely different things:

- unique plaintext passwords
- unique hash values
- rows/records
- multiple algorithms combined
- multiple source datasets
- generated candidate spaces
- historical totals rather than currently searchable records

Therefore:

> **Do not treat “20T” as automatically better than “126B” or “7.5B.”**

The real metrics are:

1. Algorithms supported
2. Actual searchable records
3. Coverage of common password populations
4. Freshness of the corpus
5. Duplicate rate
6. Exact-match hit rate
7. Batch/API limits
8. Response time
9. Privacy model
10. Current availability

---

# 9. Privacy & Safety

A hash is not automatically harmless data. Depending on the source, it may be linked to an account, email address, username, or an actual credential record.

For defensive work:

- Prefer **local/client-side identification** when possible.
- Prefer **k-anonymity/range-query** breach services for password exposure checks.
- Do not submit current plaintext passwords to public websites.
- Only analyze credential material belonging to systems/accounts you are authorized to assess.
- Treat public “crack databases” as untrusted external services.

---

# Quick Reference

## Best for huge plaintext lookup

**CMD5** → **BaconHash** → **CrackCrypt** → **Weakpass** → **Hashes.com** → **CrackStation** → **NTLM.PW**

## Best for identifying an unknown hash

**Hashes.com Identifier** → **Toolsana** → **J-Kit** → **dCode** → **BrowserUtils** → **HackUtils**

## Best for privacy

**BrowserUtils / mlab.sh / client-side tools** → **HIBP range queries** → **local tools**

## Best for forensic known-file matching

**CIRCL Hashlookup** → **HashScanner / NSRL**

## Best for local/offline advanced analysis

**mdxfind + Hashcat + local authorized datasets**

---

## Final note

The strongest setup is **not one magical lookup website**. It is a layered system:

```text
       ┌─────────────────────────┐
       │       HASH INPUT        │
       └────────────┬────────────┘
                    │
            ┌───────▼───────┐
            │ IDENTIFICATION│
            └───────┬───────┘
                    │
        ┌───────────┼───────────┐
        ▼           ▼           ▼
     LOOKUP      BREACH      FORENSICS
        │         CHECK          │
        ▼           ▼            ▼
 CMD5/Bacon/    HIBP/GWDG    CIRCL/NSRL
 Weakpass/etc.
        │
        ▼
  LOCAL AUTHORIZED
  ANALYSIS IF NEEDED
        │
        ▼
 mdxfind / Hashcat
```

**Use the public services as complementary sources, not as a single source of truth.**
