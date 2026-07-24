# documentation - GenAI general architecture



```
                              USER
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    YOUR GENAI APPLICATION                      │
├──────────────────────────────────────────────────────────────┤
│                                                                │
│  1. NeMo Input Rails (Optional)                                │
│       • Topic filtering                                        │
│       • Intent validation                                      │
│       • Domain restrictions                                    │
│       • Conversation flow                                      │
│                                                                │
│  2. ARENA SDK                                                  │
│       • Detect PII (Regex + Presidio + spaCy + Transformers)   │
│       • Card Numbers                                           │
│       • Names                                                  │
│       • Addresses                                              │
│       • SSNs                                                   │
│       • Emails                                                 │
│       • Phone Numbers                                          │
│                                                                │
│  3. PII Protection                                             │
│       • Redaction                                              │
│       • De-Risking (Preferred)                                 │
│                                                                │
│  4. SafeChain                                                  │
│       • LangChain Wrapper                                      │
│       • LCEL Support                                           │
│       • Recipe CLI                                             │
│       • Model Routing                                          │
│       • Token Management                                       │
│       • Logging                                                │
│       • Redaction Integration                                  │
│                                                                │
│  5. IDaaS Authentication                                       │
│       OAuth2 Client Credentials                                │
│                       │                                        │
│                       ▼                                        │
│               JWT Bearer Token                                 │
└──────────────────────────────────────────────────────────────┘
                               │
                       HTTPS + JWT Token
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                  EAG (Enterprise API Gateway)                  │
├──────────────────────────────────────────────────────────────┤
│                                                                │
│  Validate JWT                                                  │
│                                                                │
│  AI FIREWALL (INPUT)                                           │
│                                                                │
│  Stage 1 → Regex Rules                                         │
│  Stage 2 → ModernBERT                                          │
│  Stage 3 → GPT-5-nano (LLM-as-Judge)                           │
│  Stage 4 → Decision + Audit Logging                            │
│                                                                │
│  Detects                                                       │
│   • Prompt Injection                                           │
│   • Jailbreak Attempts                                         │
│   • Toxic Content                                              │
│   • Data Exfiltration                                          │
│   • Residual PII                                               │
└──────────────────────────────────────────────────────────────┘
                               │
                               ▼
                    Enterprise LLM Platform
         GPT-4o | GPT-5 | Llama 3.x | Embeddings
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                     AI FIREWALL (OUTPUT)                       │
├──────────────────────────────────────────────────────────────┤
│                                                                │
│  Same Pipeline                                                 │
│                                                                │
│  Regex                                                         │
│  ModernBERT                                                    │
│  GPT-5-nano                                                    │
│                                                                │
│  Detects                                                       │
│   • PII Leakage                                                │
│   • Harmful Responses                                          │
│   • Sensitive Information                                      │
└──────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                  APPLICATION POST PROCESSING                   │
├──────────────────────────────────────────────────────────────┤
│                                                                │
│  Restore Original Values (if De-Risking)                       │
│                                                                │
│  Hallucination Guardrails (Optional)                           │
│   • Empirica                                                   │
│   • RAGAS                                                      │
│   • Citation Verification                                      │
│   • Faithfulness Scoring                                       │
│                                                                │
│  NeMo Output Rails (Optional)                                  │
│   • Policy Compliance                                          │
│   • Tone                                                       │
│   • Escalation Rules                                           │
│                                                                │
│  OPA (Agentic AI Only)                                         │
│   • Tool Authorization                                         │
│   • MCP Gateway                                                │
│   • Database Access                                            │
│   • Agent Permissions                                          │
└──────────────────────────────────────────────────────────────┘
                               │
                               ▼
                              USER
```

Want this as a downloadable text/markdown file, or as an editable Mermaid diagram?
