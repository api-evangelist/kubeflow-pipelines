# Kubeflow Pipelines (kubeflow-pipelines)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Kubeflow Pipelines is a platform for building and deploying portable, scalable machine learning workflows based on Docker containers. It provides a way to orchestrate complex ML workflows with dependencies, enabling data scientists and ML engineers to deploy production-ready ML systems on Kubernetes.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/kubeflow-pipelines/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/kubeflow-pipelines/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Data Science
- Kubernetes
- Machine Learning
- MLOps
- Orchestration
- Pipelines
- Workflows

## Timestamps

- **Created:** 2024-01-01
- **Modified:** 2026-04-28

## APIs

### Kubeflow Pipelines REST API

REST API for managing ML pipelines, experiments, runs, and artifacts. Provides programmatic access to create, execute, and monitor ML workflows on a Kubeflow Pipelines deployment.

- **Human URL:** [https://www.kubeflow.org/docs/components/pipelines/reference/api/kubeflow-pipeline-api-spec/](https://www.kubeflow.org/docs/components/pipelines/reference/api/kubeflow-pipeline-api-spec/)
- **Base URL:** `https://your-kubeflow-host/pipeline`

#### Tags

- Experiments
- Pipelines
- REST API
- Runs

#### Properties

- [Documentation](https://www.kubeflow.org/docs/components/pipelines/reference/api/kubeflow-pipeline-api-spec/)
- [OpenAPI](https://raw.githubusercontent.com/kubeflow/pipelines/master/backend/api/v2beta1/swagger/pipeline.swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/kubeflow-pipelines.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/kubeflow-pipelines.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kubeflow Pipelines Python SDK

Python SDK for building, compiling, and submitting ML pipelines. Provides decorators and utilities to define pipeline components and workflows using Python.

- **Human URL:** [https://kubeflow-pipelines.readthedocs.io/](https://kubeflow-pipelines.readthedocs.io/)
- **Base URL:** `https://pypi.org/project/kfp/`

#### Tags

- Client Library
- DSL
- Python
- SDK

#### Properties

- [Documentation](https://kubeflow-pipelines.readthedocs.io/)
- [GitHub Repository](https://github.com/kubeflow/pipelines/tree/master/sdk/python)
- [Examples](https://github.com/kubeflow/pipelines/tree/master/samples)
- [Postman Collection](collections/kubeflow-pipelines.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/kubeflow-pipelines.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kubeflow Pipelines Go Client

Go client library for interacting with the Kubeflow Pipelines API programmatically from Go applications.

- **Human URL:** [https://github.com/kubeflow/pipelines/tree/master/backend/api/go_client](https://github.com/kubeflow/pipelines/tree/master/backend/api/go_client)

#### Tags

- Client Library
- Go
- SDK

#### Properties

- [Documentation](https://pkg.go.dev/github.com/kubeflow/pipelines/backend/api/go_client)
- [GitHub Repository](https://github.com/kubeflow/pipelines/tree/master/backend/api/go_client)
- [Postman Collection](collections/kubeflow-pipelines.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/kubeflow-pipelines.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Kubeflow Pipelines Metadata API

API for tracking and managing metadata about ML artifacts, executions, and lineage information throughout the ML pipeline lifecycle, backed by ML Metadata (MLMD).

- **Human URL:** [https://www.kubeflow.org/docs/components/pipelines/concepts/metadata/](https://www.kubeflow.org/docs/components/pipelines/concepts/metadata/)

#### Tags

- Artifacts
- Lineage
- Metadata
- ML Metadata

#### Properties

- [Documentation](https://www.kubeflow.org/docs/components/pipelines/concepts/metadata/)
- [GitHub Repository](https://github.com/google/ml-metadata)
- [Postman Collection](collections/kubeflow-pipelines.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/kubeflow-pipelines.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/kubeflow)
- [Website](https://www.kubeflow.org/docs/components/pipelines/)
- [Documentation](https://www.kubeflow.org/docs/components/pipelines/)
- [Getting Started](https://www.kubeflow.org/docs/components/pipelines/getting-started/)
- [Git Hub Org](https://github.com/kubeflow)
- [GitHub Repository](https://github.com/kubeflow/pipelines)
- [Blog](https://blog.kubeflow.org/)
- [Community](https://www.kubeflow.org/docs/about/community/)
- [Changelog](https://github.com/kubeflow/pipelines/releases)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
