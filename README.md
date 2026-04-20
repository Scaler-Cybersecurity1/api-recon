# API Reconnaissance Labs

Two browser-based security labs for the Scaler Cybersecurity curriculum. Both ship as a single `index.html` — no backend, no build step, no dependencies. Host anywhere that serves static files (GitHub Pages works out of the box).

**Live demo:** open `index.html` in any modern browser, or enable GitHub Pages on this repository.

---

## What these labs teach

Students have just finished hands-on API testing with Burp Suite. These two labs extend that work into **passive reconnaissance** — the skill of extracting attack-relevant information from material that looks innocuous: public API documentation and intercepted network traffic.

The labs are hash-routed inside a single page:

| Route   | Lab                                        | Tool surface                         |
| ------- | ------------------------------------------ | ------------------------------------ |
| `#home` | Lab selector                               | —                                    |
| `#q3`   | API Spec Attack Surface Mapping            | ClearPay Developer Portal (Swagger)  |
| `#q4`   | Authentication Header Analysis             | Wireshark-style packet analyzer      |

Each lab is a self-contained interface styled after the real tool a security researcher would use.

---

## Lab 1 — API Spec Attack Surface Mapping

**Scenario.** You have discovered the public OpenAPI specification for a fintech company called *ClearPay*. The spec describes six endpoints across `public`, `users`, `payments`, and `admin` groups. Your job is to audit the surface and identify three findings that represent real-world misconfigurations.

**What the lab exercises:**

- Reading an API specification for security, not functionality.
- Spotting endpoints whose authentication requirements are missing or inconsistent with their risk level.
- Recognising when an "internal" parameter has been shipped as part of a public-facing API.
- Identifying fields in response bodies that leak internal system state — keys, identifiers, processor hints — that belong behind a feature flag or admin scope.

**What you submit.** Three free-text findings:

1. The path of the endpoint that requires no authentication.
2. The name of the optional parameter that exposes internal data.
3. The name of the response field that exposes an internal system identifier.

All three must be correct to reveal the flag. One wrong entry clears the form. There are no multiple-choice options — the student must read the spec and type the exact value.

---

## Lab 2 — Authentication Header Analysis

**Scenario.** During a penetration test of *WorkBridge* (a fictional HR platform), three API requests were captured. Each request uses a different authentication scheme — a Bearer JWT, HTTP Basic Auth, and an API key passed as a URL query parameter. The lab presents the traffic inside a Wireshark-style analyzer with a working JWT decoder and Base64 decoder in a side drawer.

**What the lab exercises:**

- Decoding JWT payloads without the signing secret and interpreting the claims (including the security implication that `role: admin` is visible to anyone who intercepts the token).
- Recognising that HTTP Basic Auth is reversible Base64, not encryption, and extracting the underlying credentials.
- Understanding why API keys in URL query parameters are a design-level leak — they end up in server access logs, browser history, proxy logs, and Referer headers.
- Reading packet detail panes and hex dumps to locate the headers that matter.

**What you submit.** Three free-text findings:

1. The role granted by the JWT in request #1.
2. The full `username:password` pair exposed in the Basic Auth header in request #2.
3. The API key leaked in the URL of request #3.

Same validation rules — all three must be correct, wrong answer clears the form.

---

## Repository layout

```
.
├── index.html   # the entire application — HTML, CSS, JS in one file
└── README.md    # this file
```

No package manager, no bundler, no runtime dependencies. The page loads Google Fonts (Inter, JetBrains Mono, Titillium Web) but will work fine without network access.

---

## Running locally

```bash
# clone the repo
git clone https://github.com/Scaler-Cybersecurity1/api-recon.git
cd api-recon

# open in a browser — no server required
open index.html
```

Or serve it with any static server if you want proper `file://` behaviour avoided:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080
```

---

## Deploying to GitHub Pages

1. Push to `main`.
2. In the repository settings, enable **Pages** with source set to **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Wait ~60 seconds and the labs will be live at `https://scaler-cybersecurity1.github.io/api-recon/`.

---

## For instructors

Flag values and correct answers are obfuscated (base64) in the client-side JavaScript — sufficient for classroom use against students who aren't yet reversing frontend code, not for adversarial settings. If you need harder validation, route submissions to a server and verify there.

Each lab's learning objective is that the student can **name the artifact**, not recognise it in a list. That is why every answer input is a free-text field. Please do not replace them with dropdowns or multiple choice — it defeats the exercise.
