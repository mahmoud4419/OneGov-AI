# OneGov AI — Reference Architecture

**Package:** `PublicSectorCitizenAIDemo` · **Region target:** me-central-1 (UAE)
**Whitelabeled:** fictional "OneGov" platform, fictional "Republic of Medina". No real data.

---

## 1. Architecture at a glance

```
                         ┌─────────────────────────────────────────────┐
                         │                CITIZEN                       │
                         │   voice (AR/EN) · text · document photo      │
                         └───────────────┬─────────────────────────────┘
                                         │
                                         ▼
                   ┌──────────────────────────────────────────┐
                   │   Frontend (web / WhatsApp / kiosk)        │
                   │   Amazon CloudFront + S3 (static)          │
                   └───────────────┬────────────────────────────┘
                                   │  (Amazon API Gateway + Cognito auth)
                                   ▼
         ┌──────────────────────────────────────────────────────────────┐
         │                      INTERACTION LAYER                         │
         │                                                                │
         │  Transcribe (AR/EN speech→text)      Polly (text→AR/EN speech) │
         │  Textract (ID / bill / form OCR)                               │
         └───────────────┬────────────────────────────────────────────────┘
                         │
                         ▼
         ┌──────────────────────────────────────────────────────────────┐
         │                 ORCHESTRATION — Bedrock Agent                  │
         │                 (Amazon Nova / Bedrock FM)                     │
         │                                                                │
         │   ┌──────────────┐   reasons over intent, calls action groups │
         │   │ Guardrails   │   ← PII redaction, denied topics, scope    │
         │   └──────────────┘                                            │
         └───┬──────────────┬──────────────┬──────────────┬──────────────┘
             │              │              │              │
             ▼              ▼              ▼              ▼
     ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌─────────────────────┐
     │ Action     │  │ Action    │  │ Action    │  │ Knowledge retrieval  │
     │ Group:     │  │ Group:    │  │ Group:    │  │ (RAG)                │
     │ Licenses   │  │ Benefits  │  │ Utilities │  │                      │
     │ Visa       │  │ Health    │  │ Complaints│  │ S3 Vectors / Open-   │
     │ (Lambda)   │  │ (Lambda)  │  │ (Lambda)  │  │ Search  ◄─ policy KB  │
     └─────┬──────┘  └─────┬─────┘  └─────┬─────┘  └─────────┬───────────┘
           │               │              │                  │
           ▼               ▼              ▼                  ▼
     ┌──────────────────────────────────┐        ┌───────────────────────┐
     │  Government systems of record      │        │  Policy documents (S3) │
     │  (DynamoDB mock gov DBs)           │        │  embedded via Bedrock  │
     │  licenses · benefits · visa ·      │        │  Titan/Nova embeddings │
     │  health · utilities                │        └───────────────────────┘
     └──────────────────────────────────┘
           │
           ▼
     ┌──────────────────────────────────┐
     │  AUDIT & COMPLIANCE                │
     │  CloudTrail + DynamoDB audit log   │
     │  (every action logged, replayable) │
     └──────────────────────────────────┘
```

---

## 2. Component responsibilities

| Layer | Service | Role |
|---|---|---|
| Frontend | CloudFront + S3, API Gateway, Cognito | Delivery, routing, citizen auth |
| Speech | Amazon Transcribe | Arabic/English speech → text (streaming) |
| Speech | Amazon Polly | Text → natural Arabic/English voice (Hala/Zayd neural voices) |
| Vision | Amazon Textract | Extract fields from ID cards, bills, forms |
| Reasoning | Amazon Bedrock Agent + Nova | Intent understanding, multi-step orchestration, tool calling |
| Safety | Bedrock Guardrails | PII redaction, denied topics (e.g. medical/legal advice), scope control |
| Knowledge | **S3 Vectors or OpenSearch** (UAE) | Vector store for policy RAG — *see region note* |
| Actions | AWS Lambda (per domain) | Execute transactions against systems of record |
| Data | Amazon DynamoDB | Mock government systems of record (fictional data) |
| Audit | CloudTrail + DynamoDB | Immutable action log for compliance replay |

---

## 3. UAE region (me-central-1) data residency notes

> This is the single most important technical honesty point for Public Sector buyers in
> the UAE. Call it out proudly — it shows we know the region.

- **Bedrock Knowledge Bases — NOT yet available in me-central-1.**
  → Architect the RAG vector store on **Amazon S3 Vectors** or **Amazon OpenSearch**
  (both in-region). Embeddings generated via Bedrock (Titan/Nova embeddings) and stored
  in-region; retrieval wired into the agent as a custom retrieval action group.
- **All PII and systems-of-record stay in me-central-1** — DynamoDB, S3, OpenSearch all
  regional. No citizen data leaves the UAE.
- **Foundation models:** use models available in-region; where a specific model is only
  in a nearby region, note the cross-region inference trade-off explicitly rather than
  hiding it.
- Keep this section updated as me-central-1 service availability expands.

---

## 4. Security & governance posture (talking points)

- **Guardrails** enforce: PII redaction in logs, denied topics (no medical/legal/financial
  advice), and scope confinement (only government-service intents).
- **Least-privilege Lambda:** each action group's Lambda has IAM scoped to only its
  domain's data store.
- **Audit replay:** every agent decision + action is written to an immutable log, so a
  compliance officer can reconstruct exactly what the agent did and why.
- **Human-in-the-loop option:** high-risk actions (e.g. benefit disbursement) can be
  configured to require human approval before the Lambda commits.

---

## 5. Build phases

1. **Phase 1 (this deliverable):** brief + architecture + demo script.
2. **Phase 2:** sample data (fictional citizens, policy docs), Bedrock Agent action-group
   OpenAPI schemas, Guardrail config, mock DynamoDB seed.
3. **Phase 3:** deployable frontend (voice + chat + upload), CDK/Terraform IaC, Setup_Guide.md
   — matching the `AutomotiveDataAIDemo` / `SkyOps` package depth.

---

*Fictional/whitelabeled content only. Maintained by Mahmoud Elserafi · MENAT Industry AI Accelerator.*
