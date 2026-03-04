# left-pad — The 11 Lines That Broke the Internet

**Repository:** `left-pad/left-pad` (unpublished from npm, March 2016)
**Function:** `leftPad()`
**Lines:** 11

---

## The Code

```javascript
module.exports = leftPad;

function leftPad (str, len, ch) {
  str = String(str);
  var i = -1;
  if (!ch && ch !== 0) ch = ' ';
  len = len - str.length;
  while (++i < len) {
    str = ch + str;
  }
  return str;
}
```

---

## Why It's Here

On March 22, 2016, Azer Koçulu unpublished 273 npm packages, including `left-pad`. Within hours, builds for Babel, React, and thousands of other projects started failing worldwide. The package had 2.5 million weekly downloads.

The function itself has cyclomatic complexity of 3. It is not complex. It is 11 lines. A junior developer could write it from memory.

**That is exactly the point.**

---

## Structural Properties

| Property | Observation |
|---|---|
| **Cyclomatic complexity** | 3. Trivially simple. |
| **Coupling** | Zero internal coupling. One input, one output. |
| **Change frequency** | Effectively zero after initial publish. |
| **Test surface** | Minimal. The function is too simple to fail in isolation. |
| **Risk source** | Not the code. The *dependency graph*. |

---

## What Makes It Interesting

left-pad is the canonical example of **ecosystem-level risk** that no file-level analysis can detect.

The risk was not in the code. The risk was in:

1. Millions of projects taking a hard dependency on 11 lines they could have written inline
2. npm's publish/unpublish model having no stability guarantees
3. No organization-level resilience for a single package removal

Standard static analysis tools — and most risk metrics — would give `left-pad` a near-zero risk score. It is tiny, stable, simple, and well-defined.

And yet it broke the internet.

---

## The Risk Angle

This is the case that motivates **dependency graph analysis** as a distinct risk surface from code complexity.

A Hotspots-style analysis of a project depending on `left-pad` would not have flagged it — but a full dependency risk model would ask:

- How many of your dependencies have a single maintainer?
- Which dependencies are trivially replaceable?
- What is your build's exposure to unpublish events?

The left-pad incident pushed npm to change their unpublish policy (packages with more than 300 downloads/week can no longer be unpublished). It also pushed the JavaScript community toward vendoring and lockfiles as mandatory practice.

**The lesson:** The riskiest code in your codebase might be 11 lines you didn't write, in a package you've never looked at, maintained by one person who might be having a bad day.

---

## Further Reading

- [A discussion about left-pad](https://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm) — npm's official post-mortem
- [I'm Unpublishing left-pad](https://web.archive.org/web/20160324162112/https://medium.com/@azerbike/i-ve-just-liberated-my-modules-9045c06be67c) — Azer Koçulu's original post (archived)
- [The Node.js left-pad disaster](https://qz.com/646467/how-one-programmer-broke-the-internet-by-deleting-a-tiny-piece-of-code/) — Quartz

---

*Analyzed by [Hotspots](https://hotspots.dev) · Stephen Collins.tech LLC*
