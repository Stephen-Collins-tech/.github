# Heartbleed — The Bug That Hid in Plain Sight

**Repository:** `openssl/openssl`
**File:** `ssl/d1_both.c` (OpenSSL 1.0.1 through 1.0.1f)
**CVE:** CVE-2014-0160
**Disclosed:** April 7, 2014

---

## The Code

The vulnerable function `dtls1_process_heartbeat()` — the entire bug in context:

```c
/* Read type and payload length first */
hbtype = *p++;
n2s(p, payload);
pl = p;

/* ... */

buffer = OPENSSL_malloc(1 + 2 + payload + padding);
bp = buffer;

/* Enter response type, length and copy payload */
*bp++ = TLS1_HB_RESPONSE;
s2n(payload, bp);
memcpy(bp, pl, payload);  /* <-- here */
```

The `payload` value came directly from the client request with no bounds check against the actual length of `pl`. A client could claim a payload of 64KB and receive 64KB of server memory in response.

---

## Why It's Here

Heartbleed is not here because the code is structurally complex. It is here because it demonstrates how **complexity in the surrounding codebase creates conditions where simple bugs become catastrophic**.

The bug itself is a missing bounds check — a one-line fix. But the conditions that allowed it to exist for two years were structural:

- OpenSSL's codebase was notoriously difficult to audit (~500,000 lines, dense C macros, manual memory management)
- The heartbeat extension was added in a single commit with no formal security review
- The function mixed protocol handling, memory allocation, and response construction in one place
- No fuzzer coverage existed for the heartbeat path

---

## Structural Properties

| Property | Observation |
|---|---|
| **Cyclomatic complexity** | Low for the vulnerable function in isolation |
| **Coupling** | High. The function depended on OpenSSL's custom allocator, its TLS state machine, and its buffer abstraction. |
| **Change frequency** | Low after initial addition — the heartbeat code sat untouched |
| **Test surface** | Near zero for the heartbeat path specifically |
| **Risk source** | Not local complexity, but the *auditing cost* of the surrounding codebase |

---

## What Makes It Interesting

The Heartbleed disclosure triggered a re-evaluation of how the security community funds and audits critical infrastructure. It led directly to:

- The [Core Infrastructure Initiative](https://www.coreinfrastructure.org/) — Linux Foundation effort to fund OSS security
- LibreSSL fork — OpenBSD's attempt to produce a more auditable TLS implementation
- BoringSSL — Google's internal fork, focused on a smaller, simpler API surface
- Widespread adoption of fuzzing (AFL, libFuzzer) as a mandatory security practice

It also changed how developers think about code surface area. Every line of code in a security-critical library is an attack surface. The heartbeat extension added ~500 lines. Those 500 lines were not worth the feature.

---

## The Risk Angle

A Hotspots analysis of OpenSSL circa 2014 would have surfaced `d1_both.c` as a file worth examining — not necessarily because of the heartbeat function, but because:

- OpenSSL as a whole had extremely high coupling between modules
- Many files were changed frequently by many different contributors
- The memory management patterns were non-standard and easy to misuse

The broader lesson: **high-risk files are not just the ones with the most complex logic. They are the ones where the cost of a reviewer missing something is highest.**

Security-critical code with dense C macros, manual memory management, and no fuzzing is high-risk regardless of its cyclomatic complexity score. Any useful risk metric must account for this.

---

## The Fix

```c
/* Actual received heartbeat message length */
if (1 + 2 + 16 > s->s3->rrec.length)
    return 0; /* silently discard */

hbtype = *p++;
n2s(p, payload);

if (1 + 2 + payload + 16 > s->s3->rrec.length)
    return 0; /* silently discard per RFC 6520 sec. 4 */
```

Two bounds checks. The patch was three lines. The bug cost an estimated $500M in remediation worldwide.

---

## Further Reading

- [Heartbleed.com](https://heartbleed.com/) — the disclosure site
- [The Heartbleed Bug technical explanation](https://blog.cryptographyengineering.com/2014/04/08/attack-of-the-week-openssl-heartbleed/) — Matthew Green
- [OpenSSL: The New Face of Technology Failure](https://www.propublica.org/article/the-worlds-email-encryption-software-relies-on-one-guy-who-is-going-broke) — ProPublica on OSS funding

---

*Analyzed by [Hotspots](https://hotspots.dev) · Stephen Collins.tech LLC*
