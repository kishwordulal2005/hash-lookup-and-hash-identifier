# hash-lookup-and-hash-identifier




# Hash Lookup & Hash Identifier Toolkit

A curated collection of online **hash lookup**, **hash identification**, and **password-hash analysis** tools.

> **Primary use:** authorized security testing, CTFs, incident response, password-audit work, and recovery of credentials you are permitted to analyze.
>
> **Privacy warning:** do not paste live passwords, password-manager secrets, or production credentials into third-party services. Prefer client-side/local tools when the data is sensitive.

## Table of Contents

- [Category 1 — Hash Check / Plaintext Lookup](#category-1--hash-check--plaintext-lookup)
- [Category 2 — Hash Identifier / Type / Salt Analysis](#category-2--hash-identifier--type--salt-analysis)
- [How to choose a tool](#how-to-choose-a-tool)
- [Important: hash type vs salt detection](#important-hash-type-vs-salt-detection)
- [Recommended workflow](#recommended-workflow)
- [Privacy and operational security](#privacy-and-operational-security)
- [Notes on database size](#notes-on-database-size)

---

# Category 1 — Hash Check / Plaintext Lookup

These services compare a supplied hash against precomputed/cracked datasets. They are useful when you have an **authorized** hash and want to see whether a known plaintext match already exists.

## 👑 1. BinSec HashLookup — TOP PICK / Original Favorite

**URL:** <https://binsec.tools/lookup/hash/>

**Supported:** MD5, SHA1, NTLM  
**Database:** 7.5+ billion passwords / 3.3 TB database  
**Salt:** The documented database contains **unsalted** SHA1, MD5 and NTLM hashes.  
**Best for:** Simple, fast lookup of common legacy hashes.

BinSec's HashLookup is the closest match to the original goal of this collection. The service explicitly describes a database containing more than **7.5 billion passwords** and their unsalted SHA1, MD5 and NTLM hashes.

### Useful characteristics

- Very simple web UI.
- Dedicated algorithm selection for SHA1, MD5 and NTLM.
- Intended for pentesting / security-research workflows.
- Sources listed by the service include SecLists password sets, RockYou2021, Fortinet2021, Honeynet and other loaded password lists.

**Official page:** <https://binsec.tools/lookup/hash/>

---

## 2. BaconHash

**URL:** <https://baconhash.pw/>

**Supported:** MD5, NTLM, SHA1  
**Claimed database:** 126+ billion entries **per algorithm**  
**Batch:** Yes, one hash per line  
**Automation:** REST API / `bacon` CLI (token required)

A major alternative to BinSec for raw MD5/NTLM/SHA1 lookup. BaconHash also accepts several practical credential-dump formats, including `user:hash`, Windows domain dumps and LDAP SHA1 representations.

**API documentation:** <https://baconhash.pw/api-docs>

---

## 3. CrackCrypt

**URL:** <https://crackcrypt.com/hash-lookup>

**Supported:** MD5, SHA1, SHA256, SHA512, NTLM  
**Claimed coverage:** 30 billion lines per supported algorithm / 150 billion total  
**Public API:** Yes  
**API rate limit:** 1 request/second/IP  
**Privacy:** Hash lookup is server-side; other listed utilities are client-side.

A newer high-volume lookup option with dedicated prepared databases for five algorithms.

**API:** <https://crackcrypt.com/api>

---

## 4. Hashes.com — Lookup / Decrypt

**Lookup:** <https://hashes.com/en/decrypt/hash/>

**Supported:** Many hash formats  
**API:** Yes  
**Batch:** API supports up to 250 hashes per request  
**Salt format:** `hash[:salt]`

Hashes.com combines identification and lookup, making it useful when you are not completely sure how a stored hash was constructed.

Its API documentation also exposes the algorithm and salt in returned matches where available.

**API documentation:** <https://hashes.com/en/docs>

---

## 5. CrackStation

**URL:** <https://crackstation.net/>

**Supported:** LM, NTLM, MD2, MD4, MD5, SHA1, SHA224, SHA256, SHA384, SHA512, RIPEMD160, Whirlpool, MySQL 4.1+ and additional formats.

**Documented lookup tables:**

- MD5 + SHA1: ~190 GB / 15 billion entries
- Other supported hashes: ~19 GB / 1.5 billion entries

CrackStation is one of the classic precomputed lookup services. Its documented database is older, but it remains valuable for broad legacy-hash coverage.

**Important:** CrackStation explicitly targets **non-salted hashes**.

---

## 6. Weakpass Lookup

**URL:** <https://weakpass.com/tools/lookup>

**Claimed corpus:** 26+ billion passwords  
**Use:** Large lookup / wordlist ecosystem  
**Privacy:** The lookup page advertises client-side processing.

Weakpass is especially useful when you want a huge corpus without immediately uploading the raw hash to a remote service.

---

## 7. NTLM.pw

**URL:** <https://ntlm.pw/>

**Primary focus:** NTLM / LM / related hash lookup  
**Automation:** API available  
**Best for:** Windows credential hashes and NTLM-specific work

A specialized alternative when your dataset is primarily NT hashes.

---

## 8. OneDev Tools — Hash Lookup / Identifier

**URL:** <https://onedev.tools/hash/identifier>

Useful as a lightweight browser tool for hash identification and basic checking. It is not a substitute for a huge external corpus like BinSec, but it is convenient for quick local/browser workflows.

---

## 9. Hashes.com API

**URL:** <https://hashes.com/en/docs>

The API exposes both **lookup** and **identifier** endpoints.

Useful features include:

- Batch lookup.
- `hash[:salt]` support.
- Returned algorithm field for found hashes.
- Separate identifier endpoint.
- Extended identification mode.

This makes Hashes.com particularly useful as a bridge between Category 1 and Category 2.

---

## Lookup Quick Comparison

| Rank | Service | MD5 | SHA1 | NTLM | Other | Large Corpus | API | Salt-aware input |
|---|---|---:|---:|---:|---|---|---|---|
| 👑 1 | **BinSec HashLookup** | ✅ | ✅ | ✅ | — | ✅ 7.5B+ | — | Limited / unsalted dataset |
| 2 | **BaconHash** | ✅ | ✅ | ✅ | — | ✅ 126B+ each | ✅ | ⚠️ |
| 3 | **CrackCrypt** | ✅ | ✅ | ✅ | SHA256/SHA512 | ✅ 30B each | ✅ | ⚠️ |
| 4 | **Hashes.com** | ✅ | ✅ | ✅ | Many | ✅ | ✅ | ✅ `hash[:salt]` |
| 5 | **CrackStation** | ✅ | ✅ | ✅ | Many | ✅ 15B MD5/SHA1 table | — | ❌ Unsalted focus |
| 6 | **Weakpass** | ✅ | ✅ | ✅ | Large wordlist ecosystem | ✅ 26B+ passwords claimed | — | ❌ |
| 7 | **NTLM.pw** | — | — | ✅ | LM/SHA256-related | Specialized | ✅ | ⚠️ |

---

# Category 2 — Hash Identifier / Type / Salt Analysis

These tools answer a different question:

> **"What hashing scheme does this string look like?"**

This category is for identifying algorithms, structured password formats, parameters, encodings and—in cases where the format actually contains the information—salts and cost parameters.

## 👑 1. Hashes.com Hash Identifier — TOP PICK

**URL:** <https://hashes.com/en/tools/hash_identifier>

A strong general-purpose identifier and the best companion to BinSec in this list.

### Notable features

- Up to 25 hashes in the web UI.
- `hash[:salt]` input format.
- **Expert mode** to return all plausible possibilities.
- Large set of recognized password-hash formats.
- API identifier endpoint.
- Extended identification mode can return constructions such as salted MD5 variants and application-specific formats.

**API:** <https://hashes.com/en/docs>

---

## 2. dCode Hash Identifier

**URL:** <https://www.dcode.fr/hash-identifier>

One of the broadest online hash-identification references, covering hundreds of candidate formats and providing explanatory results.

Best for:

- Unknown hashes.
- Comparing several candidate algorithms.
- Investigating unusual legacy formats.

---

## 3. Toolsana Hash Identifier

**URL:** <https://toolsana.com/tools/hash-identifier/>

A strong modern identifier with broad password-hash coverage.

Commonly covered families include:

- MD5 / MD4 / SHA families
- NTLM
- bcrypt
- Argon2
- scrypt
- PBKDF2
- Unix `crypt`
- MySQL / MSSQL
- WordPress / Drupal / CMS-oriented formats
- LDAP and other structured representations

Useful when you need more than a simple "MD5 or SHA1?" guess.

---

## 4. WebToolMatrix Hash Identifier

**URL:** <https://webtoolmatrix.com/hash/free-online-identify-hash-type/>

Focused on algorithm recognition from string structure and characteristics such as:

- Length
- Character set
- Prefixes
- Recognizable structured formats
- Salt-bearing password formats

Useful for triaging unknown password hashes.

---

## 5. BrowserUtils Hash Identifier

**URL:** <https://www.browserutils.dev/tools/hash-identifier/>

One of the better privacy-oriented options because the site states the analysis runs in the browser.

Useful families include MD5, SHA-1, SHA-2, bcrypt, Argon2, scrypt, NTLM and other common password-hash formats.

**Best for:** Sensitive hashes where you want an identifier without uploading the value.

---

## 6. J-Kit Hash Identifier

**URL:** <https://jkit.tools/en/tools/hash-identifier>

Good at distinguishing between:

- **Certain structural matches**
- **Probabilistic candidates**

For structured formats it can expose parameters such as cost or other embedded information where the format contains it.

---

## 7. Aback Tools — Password Hash Identifier

**URL:** <https://abacktools.com/tools/crypto/utilities/password-hash-identifier>

Useful for browser-side hash identification with confidence-oriented results and support for common password hashing algorithms.

---

## 8. HackUtils Hash Identifier

**URL:** <https://hackutils.com/hash-identifier>

Particularly useful for offensive-security / CTF workflows because it can pair an identified algorithm with the corresponding **Hashcat mode**.

---

## 9. HackerDNA Hash Identifier

**URL:** <https://hackerdna.com/tools/hash-identifier>

Useful for understanding ambiguous hashes in context.

Example: a 32-character hexadecimal value can match several algorithms; database/application context can materially change the likely answer.

---

## 10. Toolali Hash Identifier

**URL:** <https://toolali.com/cryptography/hash-identifier/>

Broad hash-type detection with multiple candidates when an input is inherently ambiguous.

---

## 11. ScanSuite Hash Identifier

**URL:** <https://scansuite.io/tools/hash-identifier/>

Browser-oriented identification with candidate ranking and confidence-oriented analysis.

---

## 12. Shub Raj Apps Hash Identifier

**URL:** <https://app.shubraj.com/hash-identifier/>

Covers common digest algorithms and password-hash families such as bcrypt, Argon2, PBKDF2, scrypt and NTLM.

---

## 13. Gizza Hash Tools

**URL:** <https://gizza.ai/tools/>

Useful coverage around bcrypt, Argon2, MD5, SHA families, NTLM, `sha512crypt`, PHPass and related password formats.

---

## 14. KitLab Hash Analyzer

**URL:** <https://kitlab.app/en/tools/hash-analyzer>

More of an analyzer than a simple length checker. It can provide algorithm-family information and security-oriented interpretation.

---

## 15. Dozenkit Hash Identifier

**URL:** <https://dozenkit.com/tools/hash-identifier>

Combines hash identification with related verification tooling. Useful for browser-side experimentation and second opinions.

---

## 16. ZeroServer Tools Hash Identifier

**URL:** <https://zeroserver.tools/hash-identifier/>

Lightweight identifier with emphasis on ambiguity handling and client-side analysis.

---

## 17. Base64.sh Hash Identifier

**URL:** <https://www.base64.sh/hash-identifier/>

Quick detector for common algorithms and structured password formats.

---

## 18. Mlab.sh Hash Identifier

**URL:** <https://mlab.sh/tool/hash-identifier>

Useful browser/local-style identification and candidate ranking for ambiguous values.

---

## 19. CyberChef

**URL:** <https://gchq.github.io/CyberChef/>

Not a single-purpose hash identifier, but one of the best general-purpose analysis environments for:

- Hashing
- Encoding/decoding
- Base64 / hex conversion
- Extracting and testing transformations
- Chaining operations

**Best for:** investigating how a stored value may have been transformed before hashing.

---

## 20. Name That Hash

**Project:** Search for `Name-That-Hash` on GitHub / PyPI.

A local/offline identification approach. This is useful when you want the **identifier database on your own machine** rather than sending hashes to an online service.

---

## 21. HashID

**Project:** `hashid` / HashID tooling.

A classic offline hash-identification utility. It is lightweight and useful in CTF and pentest environments.

---

## Identifier Quick Comparison

| Rank | Identifier | Broad coverage | Salt-aware structured formats | Client-side / Local | Extra |
|---|---|---|---|---|---|
| 👑 1 | **Hashes.com** | ⭐⭐⭐⭐⭐ | ✅ | Mixed | Expert mode + API |
| 2 | **dCode** | ⭐⭐⭐⭐⭐ | ✅/candidate-based | Web | Huge reference coverage |
| 3 | **Toolsana** | ⭐⭐⭐⭐⭐ | ✅ | Web | Detailed format coverage |
| 4 | **J-Kit** | ⭐⭐⭐⭐ | ✅ | Web | Strong certainty-vs-candidate distinction |
| 5 | **BrowserUtils** | ⭐⭐⭐⭐ | ✅ | ✅ Browser | Privacy-focused |
| 6 | **WebToolMatrix** | ⭐⭐⭐⭐ | ✅/heuristic | Web | Salt/structure analysis |
| 7 | **HackUtils** | ⭐⭐⭐⭐ | ⚠️ | Web | Hashcat modes |
| 8 | **Aback Tools** | ⭐⭐⭐⭐ | ✅ | ✅ Browser | Confidence-oriented |
| 9 | **HackerDNA** | ⭐⭐⭐ | ✅/contextual | Web | Explains ambiguity |
| 10 | **Name That Hash** | ⭐⭐⭐⭐ | ✅ | ✅ Local | Offline option |

---

# Important: Hash Type vs Salt Detection

A common mistake is assuming that a hash identifier can always tell you the exact algorithm and whether a salt was used.

It cannot.

## Example: ambiguous raw digest

```text
5f4dcc3b5aa765d61d8327deb882cf99
```

This is a 32-character hexadecimal value. It is compatible with multiple possible constructions, including common MD5/MD4/NTLM-style representations.

From that value **alone**, you generally cannot prove:

- the exact algorithm;
- whether a salt was used elsewhere in the application;
- where the salt was stored;
- whether the string is a raw digest or one component of a larger password-hashing construction.

A good identifier should return **candidate algorithms** rather than fake certainty.

## Example: structured password hash

```text
$2b$12$...
```

This is structurally recognizable as bcrypt and includes its cost and salt information in the standard encoded representation.

Another example:

```text
$argon2id$v=19$m=65536,t=3,p=4$...
```

The structure identifies Argon2id and encodes parameters plus the salt representation.

### Rule of thumb

**Raw digest:** identification is often probabilistic.  
**Structured password hash:** identification can be much more certain because the format itself carries metadata.

---

# Recommended Workflow

For an authorized assessment, CTF, or password audit, use the tools in this order:

```text
                UNKNOWN HASH
                     │
                     ▼
        ┌─────────────────────────┐
        │ Category 2: IDENTIFIER  │
        │                         │
        │ Hashes.com / Toolsana   │
        │ dCode / J-Kit           │
        └────────────┬────────────┘
                     │
                     ▼
       Is the format structurally
              identifiable?
              /          \
            YES            NO
             │              │
             ▼              ▼
      Read parameters     Keep multiple
      / salt / cost       candidate types
             │              │
             └──────┬───────┘
                    ▼
         Category 1: LOOKUP
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
    BinSec       BaconHash    CrackCrypt
       │            │            │
       └────────────┼────────────┘
                    ▼
               Hashes.com
                    │
                    ▼
            Compare results
```

## Practical fallback chain

### For MD5 / SHA1 / NTLM

```text
BinSec
   ↓
BaconHash
   ↓
CrackCrypt
   ↓
Hashes.com
   ↓
CrackStation
   ↓
Weakpass
```

### For an unknown password hash

```text
Hashes.com Identifier
   ↓
Toolsana
   ↓
dCode
   ↓
J-Kit
   ↓
BrowserUtils / Aback Tools
   ↓
Choose the most defensible format
   ↓
Use the appropriate lookup / offline tool
```

---

# Privacy and Operational Security

## Never upload these to random public services

- Current personal passwords
- Password-manager vault contents
- Production user databases without authorization
- API keys
- Session secrets
- Authentication tokens
- Enterprise credential dumps

Even a password hash can be sensitive because a successful lookup may reveal the plaintext password.

## Prefer local tools when possible

For sensitive work, favor:

- Browser-only/client-side identifiers
- Name That Hash
- HashID
- CyberChef running locally
- Offline wordlists/databases

For remote lookup services, use only hashes you are authorized to submit.

---

# Notes on Database Size

Database-size claims from different services are **not directly comparable**.

For example:

```text
"7.5B passwords"
```

and

```text
"126B entries per algorithm"
```

may represent very different datasets, deduplication rules, transformations, algorithms and password sources.

A larger advertised number does **not automatically mean better coverage** for the particular hash you have.

The best way to compare services is to build a small authorized benchmark corpus and record:

- Found / not found
- Algorithm
- Plaintext correctness
- Lookup latency
- Batch support
- API availability
- Rate limits
- Privacy model
- Dataset freshness

---

# Best Picks

## 🏆 Best overall lookup stack

1. **BinSec HashLookup** — original favorite and simple MD5/SHA1/NTLM workflow.
2. **BaconHash** — extremely large claimed MD5/NTLM/SHA1 corpus.
3. **CrackCrypt** — broad five-algorithm lookup with a public API.
4. **Hashes.com** — excellent crossover between identification and lookup.
5. **CrackStation** — classic, proven unsalted-hash database.

## 🧠 Best identifier stack

1. **Hashes.com Identifier** — strongest overall fit for this workflow.
2. **Toolsana** — broad password-hash coverage.
3. **dCode** — huge format-reference coverage.
4. **J-Kit** — strong structured-format analysis.
5. **BrowserUtils** — privacy-friendly browser analysis.

## 🔒 Best privacy-oriented options

1. **BrowserUtils**
2. **Weakpass lookup** (where client-side processing is used)
3. **CyberChef local instance**
4. **Name That Hash**
5. **HashID**

---

# Important Legal / Safety Note

Use lookup and identification services only for hashes you own or are explicitly authorized to analyze—for example your own password audit, an approved penetration test, incident response, or a CTF/lab.

A hash lookup service is not "decryption" in the usual sense. Most lookup services work by searching precomputed datasets for a matching digest.

---

## Source / Verification Notes

The top BinSec entry and several service capabilities in this document were checked against the services' current public pages during **August 2026**.

- BinSec HashLookup: <https://binsec.tools/lookup/hash/>
- Hashes.com Identifier/API: <https://hashes.com/en/tools/hash_identifier> / <https://hashes.com/en/docs>
- CrackStation: <https://crackstation.net/>
- BaconHash: <https://baconhash.pw/> / <https://baconhash.pw/api-docs>
- CrackCrypt: <https://crackcrypt.com/hash-lookup> / <https://crackcrypt.com/api>

> **Last checked:** August 20, 2026
