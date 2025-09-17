# ADR-0001: LLM Provider Agnostic Architecture

## Kontext
Die Plattform orchestriert mehrere Agenten, die fuer Kunden-CI/CD Pipelines arbeiten. LLM-Faehigkeiten sind Kernbestandteil, muessen aber provider-neutral bleiben, um Kosten, Risiko und regulatorische Anforderungen zu managen. Provider-Abhaengigkeit wuerde FUehrer schnell zu Lock-in und Compliance-Problemen.

## Entscheidung
Wir bauen einen Model Router, der Self-Hosted (vLLM/TGI/Ollama) und Cloud-LLMs (Azure OpenAI, Anthropic, Google, Amazon) ueber abstrahierte Contracts ansteuert. Routing basiert auf Policies (Kosten, Risiko, Tenant, Datenklassifizierung). Der Router bietet Feature Flags, Failover und Beobachtbarkeit.

## Konsequenzen
- Pro: Flexibilitaet bei Kosten (Spot GPUs vs. SaaS), regulatorische Kontrollen (EU-only), schnelles Einbinden neuer Modelle, Failover bei Ausfaellen.
- Contra: Hoehere Komplexitaet im Gateway, Bedarf nach umfangreichen Tests, Latenz durch Routing, Pflege der Provider-SDKs.

## Status
Accepted (2025-09-17) ? umgesetzt in Gateway/Orchestrator Roadmap.

## Entscheidungsgrundlage
- Kunden fordern Provider-Wahlfreiheit, Data Souveraenitaet.
- Risikoanalyse (Risk-2025-04) identifizierte Single-Provider als High Impact.
- Early Tests zeigen vergleichbare Qualitaet bei Provider-Kombination.

## Umsetzung
- Model Router Service mit Policy Engine, Observability (Prometheus), Logging (Loki).
- Secrets/Keys via Vault, Quotas via Redis.
- Evaluation Framework (A/B Tests) ueber `md.html?path=evaluation/evaluation.md`.

## Folgeaktionen
- Erweiterung ADR sobald neue Provider (Mistral, Cohere) eingebunden.
- Regelmaessige Provider-Bewertung (Qualitaet, Kosten, Compliance).
- Schulung fuer Teams zum Umgang mit Modell-Policies.

## Links
- [Evaluation Framework](md.html?path=evaluation/evaluation.md)
- [Security Framework](md.html?path=security/security.md)
- [Risk Register](md.html?path=risk/risk.md)
- [Roadmap](md.html?path=roadmap.md)
