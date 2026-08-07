# Aragon AI Assistant — Security & Safety FAQ

**System:** Aragon Maestro | Model: Qwen (via llama.cpp) | Interface: llama-ui + custom MCP servers  
**Audience:** Clients and end users  
**Last updated:** June 2026

---

## Overview

This document answers the most common questions clients ask about the AI model used in the Aragon assistant, its safety profile, and the specific measures in place to protect user data and system integrity. It is intended to be honest, transparent, and technically accurate.

---

## Q1 — What AI model does Aragon use, and why is it sometimes described as "unsafe"?

### The Model

Aragon uses **Qwen**, a family of large language models (LLMs) developed by **Alibaba Cloud** and released as open-weight models under permissive licenses[cite:16]. The specific variant runs locally via **llama.cpp**, a widely used open-source inference engine.

### What "Unsafe" Actually Means

The term "unsafe" in AI safety literature does **not** mean the system is dangerous to use or that your data is at risk. It refers specifically to **content guardrail weaknesses** — meaning the model can sometimes be manipulated (via adversarial inputs known as "jailbreaks") into generating harmful text such as malware code, dangerous recipes, or scam content[cite:5][cite:7].

Independent evaluations have rated Qwen models as critically weak in this dimension[cite:3]. Comparable issues exist in models from DeepSeek and, to varying degrees, in Western models such as GPT-4 and Llama[cite:4]. No commercial LLM today is rated fully safe against all adversarial inputs.

### What This Means in Practice

For the Aragon deployment:

- All model inputs pass through **prompt sanitization** before reaching the model
- All model outputs are scoped through **custom MCP servers** that restrict what actions the model can take
- The model has **no internet access** during inference
- The assistant is **not suitable for safety-critical or high-risk decisions** (medical, legal, financial advice requiring regulatory compliance)

---

## Q2 — Is Qwen a Chinese model? Should I be concerned about data going to China?

### Model Origin vs. Data Destination

Qwen is developed by Alibaba Cloud, a Chinese company, and is subject to Chinese jurisdiction in its cloud form[cite:1]. However, the Aragon deployment uses the **open-weight version** of Qwen — the model weights are downloaded once and run entirely on local infrastructure[cite:36][cite:45].

This means:

- No queries, prompts, or responses are sent to Alibaba Cloud servers
- No API calls are made to any external service during inference
- Alibaba has no technical ability to access conversation data

### GDPR Implications

Because all processing is local, the deployment satisfies **GDPR Article 25** (data protection by design and by default) and avoids **Article 44** obligations on cross-border data transfers[cite:36]. The operator acts as a **user of an open-weight model**, not as a cloud service subscriber — a legally distinct and lower-risk role under the EU AI Act.

---

## Q3 — Is the system 100% safe?

### The Honest Answer

No AI system is 100% safe — and any vendor making that claim should be challenged. The Aragon assistant offers strong, demonstrable safeguards, but like all LLM-based systems it carries inherent limitations that clients should understand.

**What is guaranteed:**

- User data does not leave the local infrastructure[cite:36][cite:45]
- The model weights do not contain executable code or hidden exfiltration mechanisms — GGUF format (used by llama.cpp) is non-executable weight data[cite:40]
- Tool access (file system, APIs, shell commands) is controlled exclusively through scoped MCP servers; the system does not expose OS-level commands[cite:31]
- Model checkpoint integrity is verifiable via SHA-256 hashes matched against official Qwen releases on HuggingFace[cite:34]

**What cannot be guaranteed:**

- Factual accuracy of all responses — LLMs can hallucinate or produce plausible-sounding incorrect information
- Perfect resistance to adversarial prompts — a determined user crafting malicious inputs may elicit unexpected outputs[cite:43]
- Suitability for high-stakes decisions — the assistant is designed for general-purpose use, not for regulated or safety-critical domains

---

## Q4 — How can clients verify these safety claims?

Technical evidence is available on request for each claim:

| Claim | Verification Method |
|---|---|
| No outbound data transmission | Network traffic capture (Wireshark/tcpdump) during live inference session showing zero external connections |
| Clean model weights | SHA-256 hash of GGUF files compared against official Qwen HuggingFace checksums[cite:34] |
| Non-executable weights | llama.cpp source code review confirming GGUF is interpreted-only; no shell execution path from weights[cite:40] |
| MCP tool access is scoped | Review of MCP server configuration showing explicit permission boundaries[cite:33] |
| No OS-level shell exposure | Confirmation that llama.cpp is not running with `--tools all` flag, which would expose `exec_shell_command` and file I/O tools[cite:31] |
| Input sanitization active | Code review of prompt preprocessing pipeline before model invocation |

---

## Q5 — What risks remain, and how are they managed?

### Residual Risks

| Risk | Likelihood | Mitigation |
|---|---|---|
| Hallucination (false but confident answers) | Medium | Human review recommended for all decisions; assistant should not be sole source of truth |
| Prompt injection via adversarial user input | Low–Medium | Input sanitization layer; MCP server restricts tool scope[cite:33][cite:43] |
| Unexpected agentic behavior | Low | No autonomous internet access; tool calls require explicit MCP permission grants[cite:31] |
| Model bias or politically filtered outputs | Low (local model) | No live content filtering from Alibaba; model behavior is static post-deployment |
| GGUF supply chain compromise | Very Low | Only official Qwen releases used; checksums verified at download[cite:34][cite:40] |

### Not a Risk in This Deployment

- **Data sent to China**: Not possible — no API calls, fully offline inference[cite:36][cite:45]
- **Real-time surveillance**: The model has no persistent memory between sessions unless explicitly implemented
- **Malware in weights**: GGUF format cannot execute arbitrary code; llama.cpp is the sole runtime[cite:40]

---

## Summary for Clients

The Aragon AI assistant uses Qwen, an open-weight model running entirely on local infrastructure with no external data transmission. The label "unsafe" in AI safety benchmarks refers to content guardrail weaknesses shared by virtually all current LLMs — not to data security or surveillance risks. The local deployment architecture eliminates the primary concerns associated with Chinese-origin models. Verifiable technical evidence for all safety claims is available on request.

> **For critical decisions, always apply human judgment. The assistant is a tool, not an authority.**

