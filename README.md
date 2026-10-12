# OneGov AI — the Smart Citizen Assistant

**Package:** `PublicSectorCitizenAIDemo`
**Vertical:** Public Sector / Government Digital Services
**Part of:** MENAT Industry AI Accelerator (UseCase Repo V3)
**Agent persona:** *OneGov AI* — a single AI front door to every government service
**Status:** Demo brief · v1.0

> ⚠️ **Whitelabel notice.** This demo uses a fictional national digital-government
> platform ("OneGov") and a fictional country ("the Republic of Medina" — a made-up
> state used only for narrative). No real government, customer, citizen, or country
> data is referenced. Safe for external sharing and GitHub.

---

## 1. The one-line pitch

> *"Don't demo a chatbot. Demo a government that acts."*

OneGov AI is an **agentic, multimodal, bilingual (Arabic + English) citizen services
assistant** that doesn't just answer questions — it **gets things done**: it renews
licenses, checks benefit eligibility, files municipal complaints, tracks visa status,
and books health appointments, end-to-end, with government-grade safety, auditability,
and in-region data residency.

This re-vamps the old *"Citizen Services AI Chatbot (Arabic)"* concept. A chatbot
answers FAQs. OneGov AI **takes action on the citizen's behalf** — which is the real
story that shows AWS's agentic AI power.

---

## 2. Why this beats a "chatbot"

| Old framing (chatbot) | OneGov AI (agentic platform) |
|---|---|
| Answers FAQs | Completes transactions end-to-end |
| Text only | Voice + text + document upload (multimodal) |
| English, or clunky Arabic | Native Arabic (incl. Gulf dialect) + English |
| Ungrounded, hallucinates | Grounded + cited from policy knowledge base |
| No governance story | Guardrails, PII redaction, full audit trail |
| Region-agnostic hand-wave | Architected for in-region data residency |

---

## 3. AWS AI service showcase

| Capability | AWS Service | Citizen "wow" moment |
|---|---|---|
| Agentic actions (orchestration) | **Amazon Bedrock Agents / AgentCore** | Renews a license or files a request end-to-end — not just a reply |
| Grounded, cited answers | **Bedrock Knowledge Bases** → *in UAE: S3 Vectors or OpenSearch as vector store* | Answers from real policy docs, with citations, no hallucination |
| Bilingual, dialect-aware LLM | **Amazon Nova / Bedrock foundation models** | Native Arabic (Gulf dialect) + instant EN⇄AR translation |
| Voice in / voice out | **Amazon Transcribe + Amazon Polly** | Citizen speaks Arabic, hears a natural voice reply — accessibility & low-literacy inclusion |
| Document intelligence | **Amazon Textract + Bedrock** | Snap a photo of an ID/bill/form → auto-extract → pre-fill the application |
| Safety & governance | **Bedrock Guardrails** | PII redaction, topic/scope control, denied-topic blocking |
| Audit & compliance | **CloudTrail + DynamoDB audit log** | Every agent action is logged and replayable for government compliance |
| Data residency | **me-central-1 (UAE) architecture** | Everything stays in-region — a hard Public Sector requirement |

> **UAE region note (me-central-1):** Amazon Bedrock Knowledge Bases are **not yet
> available in me-central-1**. This demo architects the vector store on **S3 Vectors
> or Amazon OpenSearch** instead, which are in-region. This is called out explicitly in
> `ARCHITECTURE.md` so the demo stays technically honest for UAE Public Sector buyers.

---

## 4. Scenarios covered (the "showreel")

The demo walks five citizen journeys. Each is a signature "moment" designed to land a
specific AWS capability.

1. **Licenses & permits — "Renew my driving license"**
   Spoken in Arabic → agent checks eligibility → pulls the record → processes the
   renewal → confirms by voice. *(Bedrock Agents + Transcribe + Polly)*

2. **Social support / benefits eligibility — "What support am I eligible for?"**
   Grounded, cited answer from the policy knowledge base, personalized to the citizen's
   profile. *(Knowledge Bases / S3 Vectors + Nova)*

3. **Utilities & municipal complaints — "My street light is broken / dispute this bill"**
   Snap a photo of the utility bill → Textract extracts it → agent files the
   complaint/dispute automatically and returns a ticket number. *(Textract + Bedrock Agents)*

4. **Visa / residency / immigration — "What's my residency application status?"**
   Agent authenticates the citizen, checks status across systems, explains next steps
   and required documents in plain Arabic. *(Bedrock Agents + Knowledge Bases)*

5. **Health appointments & records — "Book me a clinic appointment"**
   Agent finds the nearest clinic with availability, books it, and sends confirmation —
   with a guardrail that refuses to give medical advice. *(Bedrock Agents + Guardrails)*

**Governance moment (shown across scenarios):** a live demonstration of Guardrails
blocking an out-of-scope / PII-leaking request, proving government-grade safety.

---

## 5. Package contents

```
PublicSectorCitizenAIDemo/
├── README.md               ← this file (demo brief)
├── ARCHITECTURE.md         ← reference architecture + UAE region notes
├── Demo_Script.md          ← step-by-step presenter runbook (5 scenarios)
└── (future build phase)
    ├── sample_data/        ← fictional citizen profiles, policy docs, forms
    ├── agent/              ← Bedrock Agent action-group definitions (OpenAPI)
    ├── guardrails/         ← Guardrail config (PII, denied topics)
    └── frontend/           ← demo UI (voice + chat + upload)
```

This deliverable is the **brief + architecture + script** phase. The buildable package
(code, sample data, deployable frontend) is the next phase, modeled on
`AutomotiveDataAIDemo` / `AirlineDataAIDemo (SkyOps)`.

---

## 6. Positioning notes for sellers

- **Lead with the transaction, not the chat.** The demo's power is that the citizen
  *finishes a government task* without a human agent and without a portal maze.
- **Inclusion angle:** voice-first Arabic serves low-literacy and elderly citizens — a
  strong public-sector social-value message.
- **Trust angle:** citations + guardrails + audit trail answer the first question every
  government buyer asks: *"How do I know it won't make things up or leak data?"*
- **Residency angle:** the in-region architecture (S3 Vectors/OpenSearch in me-central-1)
  pre-empts the data-sovereignty objection.

---

*Maintained by Mahmoud Elserafi · MENAT Industry AI Accelerator · fictional/whitelabeled demo content only.*
