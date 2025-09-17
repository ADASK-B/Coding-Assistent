# ADR-0002: Retrieval Augmented Generation vs. Context Only

## Kontext
Agenten benoetigen Quellcode- und Dokumentationskontext. Erste Iterationen nutzten ausschliesslich Kontext-Fenster. Mit Wachstum der Repos steigen jedoch Kontextgroesse, Kosten und Latenz. RAG bietet verlaesslichere Kontextbereitstellung.

## Entscheidung
Wir fuehren Retrieval Augmented Generation (RAG) mit Qdrant Vectorstore ein. Kontext-only bleibt fuer Low-Risk/Small Scope Aufgaben. Router entscheidet je nach Task-Typ, Tenant-Richtlinie und Kostenschwellwert. RAG Pipelines nutzen Embeddings aus Azure OpenAI oder lokalen Modellen.

## Konsequenzen
- Pro: Hohe Trefferquote bei Code-Findung, geringere Prompt-Groesse, Wiederverwendbarkeit von Kontext, Auditing moeglich.
- Contra: Betrieb weiterer Services (Indexer, Vector DB), initialer Aufbauaufwand, Data Privacy Checks fuer Embeddings.

## Status
Accepted (2025-09-17) ? Rollout geplant fuer Q4 2025.

## Entscheidungsgrundlage
- Benchmark (Evaluation-2025-02) zeigte +25% Genauigkeit bei Tests.
- Kostenanalyse ergab 30% Einsparung bei Prompt Tokens.
- Compliance Freigabe fuer Embedding Storage (verschluesselt, pseudonymisiert).

## Umsetzung
- Indexer Service (Rust/TS) mit Commit Hooks, Observability via Prometheus.
- Vectorstore Qdrant (managed) mit RBAC, TLS, Multi-Tenant Collections.
- Retriever API fuer Orchestrator und Agents.

## Folgeaktionen
- Data Masking und Consent Mechanismen fuer Embeddings finalisieren.
- Integration in Quality & Evaluation Tests (Golden Tasks).
- Schulung fuer Dev-Agent-Betreiber bzgl. Fehlermodi.

## Links
- [Data Layer](md.html?path=data/data.md)
- [Quality Framework](md.html?path=quality/quality.md)
- [Evaluation Framework](md.html?path=evaluation/evaluation.md)
- [Risk Register](md.html?path=risk/risk.md)
