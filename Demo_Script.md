# OneGov AI — Demo Script (Presenter Runbook)

**Package:** `PublicSectorCitizenAIDemo` · **Duration:** ~15 min · **Audience:** Public Sector decision-makers
**Whitelabeled:** fictional "OneGov" platform, fictional "Republic of Medina", fictional citizen "Amina Al-Rashid". No real data.

---

## 0. Setup (before the room)

- Open the OneGov AI demo UI (voice + chat + document upload enabled).
- Have the demo citizen persona ready: **Amina Al-Rashid**, a fictional resident with a
  driving license due for renewal, an open utility account, and a pending residency app.
- Mic working (voice scenarios). Sample bill photo + sample ID photo on the desktop.
- Set language toggle to **Arabic** to open; you'll show the EN⇄AR switch live.

---

## 1. Opening (60 sec) — reframe the category

> "Most 'citizen chatbots' just answer FAQs. Today I'll show you something different —
> an AI assistant that doesn't just *talk*, it *acts*. It renews licenses, files
> complaints, checks eligibility and books appointments — in Arabic or English, by
> voice or text, and it does it with government-grade safety and full data residency
> inside the UAE. We call it **OneGov AI — one AI front door to every government service**."

**Land this line:** *"A chatbot answers. OneGov AI gets it done."*

---

## 2. Scenario 1 — Licenses & permits (voice, Arabic) · ~2.5 min

**AWS stars:** Bedrock Agents · Transcribe · Polly · Nova

1. **Speak in Arabic:** *"جدد رخصة القيادة الخاصة بي"* ("Renew my driving license").
2. Show **Transcribe** turning speech → text live.
3. The agent authenticates Amina, checks renewal eligibility against the (mock) licenses
   DB, states the fee, and asks for confirmation — **in Arabic**.
4. Confirm. Agent processes the renewal, returns a reference number, and **Polly speaks
   the confirmation back** in a natural Arabic voice.

**Say:** "Notice it didn't hand her a form or a phone number — it completed the
transaction. That's the Bedrock Agent orchestrating real actions, not retrieving text."

---

## 3. Scenario 2 — Social support / benefits eligibility (grounded RAG) · ~2.5 min

**AWS stars:** Knowledge Bases / S3 Vectors · Nova · citations

1. Type or say: *"What financial support am I eligible for?"*
2. Agent retrieves from the **policy knowledge base** and gives a personalized, **cited**
   answer ("per Social Support Policy §4.2…").
3. Click a citation → show the exact source passage.

**Say:** "Every claim is grounded in real policy documents and cited — so a citizen, and
an auditor, can trust it. In the UAE we run this vector store on S3 Vectors or OpenSearch,
fully in-region, because Bedrock Knowledge Bases aren't in me-central-1 yet. We architect
for residency from day one."

---

## 4. Scenario 3 — Utilities & municipal complaint (document upload) · ~2.5 min

**AWS stars:** Textract · Bedrock Agents

1. **Upload a photo** of a (fictional) utility bill with a disputed charge.
2. **Textract** extracts account number, amount, billing period — show the fields pop in.
3. Say: *"I want to dispute this charge."* Agent files the dispute against the utilities
   system and returns a ticket number.

**Say:** "She snapped a photo — no typing an account number off a bill. Textract reads it,
the agent files the dispute. Multimodal in, action out."

---

## 5. Scenario 4 — Visa / residency status · ~2 min

**AWS stars:** Bedrock Agents · Knowledge Bases

1. Say: *"What's the status of my residency application?"*
2. Agent authenticates, checks status across the (mock) immigration system, explains the
   current stage and **exactly which documents are still needed**, in plain Arabic.

**Say:** "It's not just status — it tells her the *next step*. That deflects a call-center
contact and a physical visit."

---

## 6. Scenario 5 — Health appointment + the Guardrails moment · ~2.5 min

**AWS stars:** Bedrock Agents · **Guardrails**

1. Say: *"Book me a clinic appointment this week."* Agent finds the nearest clinic with
   availability, books it, confirms by voice.
2. **Now trigger the guardrail:** ask *"I have chest pain, what medication should I take?"*
   → the agent **refuses to give medical advice**, and instead directs her to emergency
   services / a clinician.
3. **Optional:** ask something with PII and show it **redacted** in the audit log.

**Say:** "This is the question every government buyer asks first: how do I stop it going
off-script or leaking data? Bedrock Guardrails — denied topics, PII redaction — and every
action is logged to an immutable audit trail you can replay for compliance."

---

## 7. Close (60 sec)

> "In fifteen minutes, OneGov AI renewed a license, checked benefits with citations, filed
> a utility dispute from a photo, explained a residency application, and booked a health
> appointment — in Arabic, by voice, safely, and entirely in-region. This is AWS agentic
> AI applied to the citizen. Not a chatbot — a government that acts."

**Leave-behind:** README.md (demo brief) + ARCHITECTURE.md (incl. UAE residency notes).

---

## Appendix — scenario → AWS service cheat sheet

| # | Scenario | Primary services | Signature "wow" |
|---|---|---|---|
| 1 | License renewal | Bedrock Agents, Transcribe, Polly, Nova | Voice-driven transaction completed |
| 2 | Benefits eligibility | KB / S3 Vectors, Nova | Cited, grounded, personalized answer |
| 3 | Utility complaint | Textract, Bedrock Agents | Photo → extracted → dispute filed |
| 4 | Visa/residency status | Bedrock Agents, KB | Status + exact next steps |
| 5 | Health booking + safety | Bedrock Agents, Guardrails | Books appt; refuses medical advice |

---

*Fictional/whitelabeled content only. Maintained by Mahmoud Elserafi · MENAT Industry AI Accelerator.*
