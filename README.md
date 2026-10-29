<!--
  Profile README for github.com/AvinashKalamati
  · Artwork lives in assets/ (hero + whoami cards, dark/light variants).
  · Optional later: a daily readme-sync workflow to refresh the "Recently shipped" table,
    and the snake workflow that renders the contribution animation to the `output` branch.
-->

<a href="https://www.linkedin.com/in/avinashkalamati">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/hero-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/hero-light.svg">
    <img alt="Avinash Kalamati, Software Development Engineer. I build backend services and LLM systems, and contribute to the open-source tools behind them." src="assets/hero-dark.svg" width="100%">
  </picture>
</a>

<p align="center">
  <a href="https://www.linkedin.com/in/avinashkalamati"><img alt="LinkedIn" src="https://img.shields.io/badge/LinkedIn-0A0B0D?style=for-the-badge&logo=linkedin&logoColor=FF8000"></a>
  <a href="https://github.com/AvinashKalamati"><img alt="GitHub" src="https://img.shields.io/badge/GitHub-0A0B0D?style=for-the-badge&logo=github&logoColor=FF8000"></a>
  <a href="https://www.linkedin.com/in/avinashkalamati"><img alt="Open to software engineering roles" src="https://img.shields.io/badge/Open_to-SWE_roles-0A0B0D?style=for-the-badge&logoColor=FF8000"></a>
  <a href="mailto:avinashkalamati@gmail.com"><img alt="Email" src="https://img.shields.io/badge/Email_me-0A0B0D?style=for-the-badge&logo=gmail&logoColor=FF8000"></a>
</p>

<p align="center">
  <kbd><a href="#whoami">whoami</a></kbd>&nbsp;
  <kbd><a href="#open-source">open source</a></kbd>&nbsp;
  <kbd><a href="#recently-shipped">recently shipped</a></kbd>&nbsp;
  <kbd><a href="#how-i-build">how I build</a></kbd>&nbsp;
  <kbd><a href="#experience">experience</a></kbd>&nbsp;
  <kbd><a href="#stack">stack</a></kbd>&nbsp;
  <kbd><a href="#activity">activity</a></kbd>
</p>

## whoami

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/whoami-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="assets/whoami-light.svg">
  <img alt="Role: Software Development Engineer at Quadrant Technologies (client: Microsoft), Redmond WA. Focus: backend services, distributed systems, AI infrastructure. Stack: Python, Java, C#, TypeScript, Azure, AWS, Docker, Kubernetes. Open source: testcontainers-python, Azure AI Search demo, llama-agents, LangGraph. Education: M.S. in Data Science, Clarkson University, GPA 3.8. Status: open to software engineering opportunities." src="assets/whoami-dark.svg" width="100%">
</picture>

## Open source

I contribute to the Python and AI-infrastructure tools I build on — real fixes and features, each shipped with tests, in projects with active maintainer review.

### Featured: `GcsContainer` for testcontainers-python

A new container module that runs Google's `fake-gcs-server` emulator in tests, so GCS code can be integration-tested without cloud credentials. Modeled on the repo's existing Datastore pattern, with docs and tests included.

<img alt="Pull request 1145" src="https://img.shields.io/badge/PR-%231145-0A0B0D?style=flat-square&labelColor=0A0B0D&color=FF8000">
<img alt="Maintainer approved" src="https://img.shields.io/badge/review-approved-0A0B0D?style=flat-square&labelColor=0A0B0D&color=FF8000">
<img alt="Validated downstream" src="https://img.shields.io/badge/downstream-validated-0A0B0D?style=flat-square&labelColor=0A0B0D&color=FF8000">
<img alt="Tests included" src="https://img.shields.io/badge/tests-included-0A0B0D?style=flat-square&labelColor=0A0B0D&color=FF8000">

```python
from testcontainers.community.gcs import GcsContainer

with GcsContainer() as gcs:
    client = gcs.get_client()
    bucket = client.create_bucket("test-bucket")
    bucket.blob("hello.txt").upload_from_string("Hello from the emulator!")
```

**Why it holds up:** a downstream project (panodata/omniload) swapped it in as a drop-in replacement for its own hand-rolled container — all 3 of its GCS integration cases passed unchanged, at the same runtime.

### Contributions

| Project | What I shipped | Status |
| --- | --- | --- |
| [testcontainers-python](https://github.com/testcontainers/testcontainers-python/pull/1145) | New `GcsContainer` module (fake-gcs-server) with docs and tests | Maintainer-approved, awaiting CI/merge |
| [azure-search-openai-demo](https://github.com/Azure-Samples/azure-search-openai-demo/pull/3250) | Fix: results without a reranker score were treated as 0 against the default 1.9 threshold, dropping every result when the semantic ranker was unavailable | All 21 CI checks green, awaiting review |
| [llama-agents](https://github.com/run-llama/llama-agents/pull/838) | Root-cause fix for `PickleSerializer` returning wrong values for JSON payloads, with regression tests | Open, awaiting review |
| [langgraph](https://github.com/langchain-ai/langgraph/issues/9475) | Fix in progress: Functional API resume re-runs a task whose exception was caught instead of replaying its recorded outcome | Claimed, building |

### Recently shipped

| Date | Work |
| --- | --- |
| `2026-10-28` | Claimed langgraph #9475 — checkpoint task-replay bug in the Functional API |
| `2026-10-27` | Opened llama-agents PR #838 — PickleSerializer round-trip fix (+203/−4, regression tests) |
| `2026-10-26` | Opened testcontainers-python PR #1145 — new `GcsContainer` module (+262/−2) |
| `2026-10-23` | Opened azure-search-openai-demo PR #3250 — reranker-score filtering fix |

## How I build

```
flowchart LR
    BUG["Bug report"] --> REPRO["Reproduce locally"]
    REPRO --> ROOT["Find the root cause"]
    ROOT --> FIX["Fix + regression tests"]
    FIX --> PR["Pull request"]
    PR --> REV["Maintainer review"]
    REV --> MERGE["Merged"]
```

| Principle | What it looks like in practice |
| --- | --- |
| **Root cause over patches** | In llama-agents, the serializer bug had narrower fixes that only tightened validation; mine corrects the decode ordering that caused it, so the whole class of bad round-trips is covered. |
| **Reproduce first** | Every fix starts from the reporter's repro, run locally — no repro, no patch. |
| **Tests ship with the fix** | Regression tests land in the same PR (#838, #1145), so the bug stays fixed. |
| **Fit the codebase** | New modules follow the repo's own patterns — `GcsContainer` mirrors testcontainers' Datastore module — and PRs include docs, changelog entries and CLA housekeeping. |

## Experience

**Software Development Engineer · Quadrant Technologies (client: Microsoft) · *Jun 2025 – present · Redmond, WA***

- Built RAG-based AI agents on Azure AI Foundry with LangGraph and MCP, retrieving from Microsoft Fabric and Azure AI Search — cut manual lookup time 30% for the team using them.
- Own a reusable Python/Airflow framework running 70+ production workflows on PostgreSQL, MongoDB and cloud storage — design docs, design reviews, on-call, incident response.
- Drove C#/.NET compatibility tooling that catches Windows runtime regressions across 23 device configurations — runtime issues down 27% (300 → 219/month), root-caused via Event Logs and Win32 API traces.
- Automated DEV/UAT/PROD environment sync and database refresh — drift defects down 40% per release; SonarQube/Bandit quality gates and Grafana alerts cut deployment errors 25%.

**Co-founder · Doppular**

- Co-founded Doppular, a social app that matches you with people around the world who look like you — *alike in looks, unique in life.*
- Own the multi-tier distributed backend (Node.js, TypeScript, MongoDB, Redis, BullMQ) sustaining 100K+ API requests/day, plus a Python FastAPI + Milvus vector-search microservice running sub-200ms, on AWS with Docker and Kubernetes.

**Software Engineer Intern · Skipline · *May 2024 – May 2025 · Remote***

- Owned telemetry ingestion services processing 1M+ daily records; validation cut ingestion failures 30%, and date-partitioned PySpark/SQL pipelines cut processing time 20%.

**Research Assistant · Clarkson University · *Sep 2023 – May 2024***

- Built PyTorch/scikit-learn pipelines for satellite and UAV multispectral imagery (up to 98.4% balanced accuracy); published in *Machine Learning with Applications*.

**Education:** M.S. in Data Science, Clarkson University (GPA 3.8) · B.Tech in Computer Science, JNTUK

## Stack

| Layer | Tools |
| --- | --- |
| **Languages** | Python · Java · C# · C++ · TypeScript · SQL |
| **Backend & Data** | REST APIs · Microservices · Kafka · Redis · PostgreSQL · MongoDB · Apache Airflow · Node.js · FastAPI |
| **Cloud & DevOps** | AWS (EC2, S3, Lambda, CloudFront) · Azure DevOps · Docker · Kubernetes · Grafana · CI/CD · Git |
| **AI & Agents** | RAG · AI Agents · MCP · LangGraph · Azure AI Foundry · Azure AI Search · GenAI |

<img alt="Python, Java, C#, C++, TypeScript, Node.js, FastAPI, PostgreSQL, MongoDB, Redis, Kafka, AWS, Azure, Docker, Kubernetes, Git" src="https://skillicons.dev/icons?i=python,java,cs,cpp,ts,nodejs,fastapi,postgres,mongodb,redis,kafka,aws,azure,docker,kubernetes,git&perline=8&theme=dark">

## Activity

<!-- The snake needs the Platane/snk workflow writing to the `output` branch; add it when ready.
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/AvinashKalamati/AvinashKalamati/output/github-contribution-grid-snake-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/AvinashKalamati/AvinashKalamati/output/github-contribution-grid-snake.svg">
  <img alt="Contribution graph snake" src="https://raw.githubusercontent.com/AvinashKalamati/AvinashKalamati/output/github-contribution-grid-snake-dark.svg">
</picture>
-->

<p>
  <img alt="GitHub stats" src="https://github-readme-stats.vercel.app/api?username=AvinashKalamati&show_icons=true&bg_color=0A0B0D&title_color=FF8000&icon_color=FF8000&text_color=9FB3C8&border_color=2E2A26">
  <img alt="GitHub contribution streak" src="https://streak-stats.demolab.com/?user=AvinashKalamati&background=0A0B0D&border=2E2A26&stroke=2E2A26&ring=FF8000&fire=FF8000&currStreakNum=EDEFF2&sideNums=EDEFF2&currStreakLabel=FF8000&sideLabels=8B93A0&dates=5C6670">
</p>

**Let's build systems that ship.** Open to software engineering roles — backend, distributed systems, AI infrastructure.

<a href="https://www.linkedin.com/in/avinashkalamati"><img alt="Connect on LinkedIn" src="https://img.shields.io/badge/Connect-LinkedIn-0A0B0D?style=for-the-badge&logo=linkedin&logoColor=FF8000"></a>

<img alt="Profile views" src="https://komarev.com/ghpvc/?username=AvinashKalamati&color=FF8000&style=flat-square&label=profile+views">
