---
title: "GSoC 2025: Meet Our Projects and Contributors 🚀"
url: "https://blog.kubeflow.org/gsoc/community/kubeflow/2025/09/06/kubeflow-and-gsoc2025.html"
date: "2025-09-06T00:00:00-05:00"
author: "Kubeflow Outreach Team"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<h2 id="introduction">Introduction</h2>

<p>Google Summer of Code (GSoC) 2025 has been an exciting journey for the Kubeflow community! We are very grateful for Google and the open source community members dedication and effort.🎉<br />
This year, 9 contributors from around the world collaborated with mentors to improve different parts of the Kubeflow ecosystem — from infrastructure and CI/CD, to notebooks, ML workflows, and beyond.</p>

<p>In this blog, we are highlighting all the projects that were part of <strong>GSoC 2025</strong>, their goals, the impact they’ve created, and the amazing contributors behind them.</p>

<p>👉 You can explore the full list on our <a href="https://www.kubeflow.org/events/gsoc-2025/">GSoC 2025 page</a>.</p>

<hr />

<h2 id="-project-highlights">📚 Project Highlights</h2>

<p>Below are the projects from this year’s GSoC. Each section includes a short summary, contributor details, and links to project resources.</p>

<hr />

<h3 id="project-1-kubeflow-platform-enhancements">Project 1: Kubeflow Platform Enhancements</h3>
<p><strong>Contributor:</strong> Harshvir Potpose (<a href="https://github.com/akagami-harsh">@akagami-harsh</a>)
<strong>Mentors:</strong> Julius von Kohout (<a href="https://github.com/juliusvonkohout">@juliusvonkohout</a>)</p>

<p><strong>Overview:</strong><br />
We need an up to date S3 storage with hard multi-tenancy and run our containers with PodSecurityStandards restricted. MinIO transitioned to the AGPLv3 license in 2021, creating significant compliance challenges for the project.</p>

<p>This project addressed this critical blocker by implementing SeaweedFS as a production-ready replacement for MinIO. SeaweedFS offers a more permissive Apache 2.0 license while providing superior performance characteristics and enterprise-grade security and reliability.</p>

<p><strong>Key Outcomes:</strong></p>
<ul>
  <li>Provided S3 storage with hard multi-tenancy</li>
  <li>Successfully migrated to SeaweedFS as a secure replacement for MinIO and integrated it into Kubeflow Pipelines</li>
  <li>Eliminated MinIO’s licensing constraints by adopting SeaweedFS’s more permissive license model</li>
  <li>Implemented comprehensive CI tests for SeaweedFS deployment and namespace isolation functionality</li>
  <li>Strengthened the manifests repository’s CI pipeline and contributed to the dashboard migration efforts</li>
  <li>Enforcing PodSecurityStandards baseline/restricted</li>
</ul>

<p><strong>Resources:</strong></p>
<ul>
  <li>📄 <a href="https://summerofcode.withgoogle.com/programs/2025/projects/PWDq4Zvt">Project Page</a></li>
  <li>✍️ <a href="https://medium.com/@hpotpose26/kubeflow-pipelines-embraces-seaweedfs-9a7e022d5571">Personal Blog: Kubeflow Pipelines Embraces SeaweedFS</a></li>
</ul>

<hr />

<h3 id="project-2-kserve-models-web-application-modernization">Project 2: KServe Models Web Application Modernization</h3>
<p><strong>Contributor:</strong> (GitHub: <a href="https://github.com/LogicalGuy77">@LogicalGuy77</a>)<br />
<strong>Mentors:</strong> Griffin Sullivan (<a href="https://github.com/Griffin-Sullivan">@Griffin-Sullivan</a>), Julius von Kohout (<a href="https://github.com/juliusvonkohout">@juliusvonkohout</a>)</p>

<p><strong>Overview:</strong><br />
This project revived and modernized the KServe Models Web Application (Angular + Flask), the UI used to manage machine learning inference services in Kubeflow via KServe. What began as a small Node.js update evolved into a comprehensive upgrade of the frontend stack, CI/CD, testing, and feature set—bringing the app up to modern standards and making it easier for both users and contributors to work with.</p>

<p><strong>Key Outcomes:</strong></p>
<ul>
  <li>Modernized core stack: upgraded Node.js (v16 → v23) and Angular (v12 → v14), resolving security issues and improving performance</li>
  <li>Migrated container images from Docker Hub to GitHub Container Registry (GHCR) to avoid rate limits and improve reliability</li>
  <li>Overhauled CI/CD with GitHub Actions: updated actions, added intelligent caching for pip, Docker layers, and node_modules for significantly faster builds</li>
  <li>Introduced Jest unit tests for core utilities (e.g., parsing Kubernetes object statuses and KServe predictor configs)</li>
  <li>Added Cypress end-to-end tests for critical user journeys (deploy, edit, delete) including failure handling and input validation</li>
  <li>Wrote comprehensive documentation to help contributors run and extend the test suites</li>
  <li>Shipped “Edit InferenceService YAML” directly in the UI via an integrated Monaco editor—no kubectl required</li>
  <li>Fixed RawDeployment-mode crash and added ModelMesh support so resources and statuses render correctly</li>
  <li>Added support for the latest KServe predictor runtimes, including HuggingFace</li>
  <li>Simplified contributor onboarding with a Makefile that automates full frontend setup in a single command</li>
  <li>Implemented runtime-configurable settings via a new <code class="language-plaintext highlighter-rouge">/api/config</code> endpoint (e.g., Grafana DB names, URL prefixes)</li>
  <li>Cut the v0.15.0 release of the Models Web App, consolidating months of modernization and feature work</li>
</ul>

<p><strong>By the Numbers:</strong></p>
<ul>
  <li>PRs merged: 19</li>
  <li>Issues closed: 8</li>
  <li>Lines of code changed: +22,309 / −11,628</li>
  <li>Frontend: Angular, TypeScript, SCSS</li>
  <li>Backend: Flask (Python)</li>
  <li>CI/CD: GitHub Actions, Docker</li>
  <li>Local cluster: Kubernetes (Kind) + Istio + Kubeflow</li>
</ul>

<p><strong>Resources:</strong></p>
<ul>
  <li><a href="https://github.com/kserve/models-web-app">Project Repo: kserve/models-web-app</a></li>
  <li><a href="https://github.com/kserve/models-web-app/commits?author=LogicalGuy77">All commits by @LogicalGuy77</a></li>
  <li><a href="https://medium.com/@harshitweb3/my-gsoc-2025-journey-reviving-kserves-models-web-application-2f18ef16fb51">Blog Post</a></li>
</ul>

<hr />

<h3 id="project-3-istio-cni-and-ambient-mesh">Project 3: Istio CNI and Ambient Mesh</h3>
<p><strong>Contributor:</strong> Ayush Gupta (GitHub: <a href="https://github.com/madmecodes">@madmecodes</a>)<br />
<strong>Mentors:</strong> Julius von Kohout (<a href="https://github.com/juliusvonkohout">@juliusvonkohout</a>), Kimonas Sotirchos (<a href="https://github.com/kimwnasptd">@kimwnasptd</a>)</p>

<p><strong>Overview:</strong><br />
This GSoC 2025 project modernized Kubeflow’s service mesh infrastructure by implementing Istio CNI as the default configuration and pioneering Istio Ambient Mesh support. The 175-hour medium-difficulty project involved 25+ pull requests across multiple Kubeflow repositories, transitioning from traditional sidecar-based architecture to ambient mesh with ztunnel and waypoint proxies, pioneering the migration to Gateway API (HTTPRoute), implementing path-based routing for KServe model serving endpoints, and utilizing Kustomize overlay method for easy installation and configuration management.</p>

<p><strong>Key Outcomes:</strong></p>
<ul>
  <li>Implemented Istio CNI by default with Kustomize overlay method enabling easy switching between traditional Istio and CNI configurations</li>
  <li>Created path-based routing for KServe multi-model serving and Gateway API (HTTPRoute) migration</li>
  <li>Pioneered Ambient Mesh support with ztunnel/waypoint proxies and coordinating cross-repository compatibility</li>
</ul>

<p><strong>Resources:</strong></p>
<ul>
  <li>📄 <a href="https://summerofcode.withgoogle.com/programs/2025/projects/WAHCCi8V">Project Page</a></li>
  <li>✍️ <a href="https://medium.com/@ayushguptadev1/gsoc25-kubeflow-securing-and-optimizing-ml-infrastructure-with-istio-31f535c77fd6">Blog Post</a></li>
</ul>

<hr />

<h3 id="project-4-deploying-kubeflow-with-helm-charts">Project 4: Deploying Kubeflow with Helm Charts</h3>

<p><strong>Contributor:</strong> Kunal Dugar (<a href="https://github.com/kunal-511">@kunal-511</a>)<br />
<strong>Mentors:</strong> Julius von Kohout (<a href="https://github.com/juliusvonkohout">@juliusvonkohout</a>), Valentina Rodriguez Sosa (<a href="https://github.com/varodrig">@varodrig</a>), Chase Cadet (<a href="https://github.com/Chasecadet">@Chasecadet</a>)</p>

<p><strong>Overview:</strong><br />
This project focused on creating component-based Helm charts for Kubeflow, enabling flexible and incremental deployment of ML infrastructure. Instead of requiring a full platform installation, users can now deploy specific components like Katib, Pipelines, Model Registry, and others independently with customized configurations.</p>

<p><strong>Key Outcomes:</strong></p>
<ul>
  <li>Kubeflow AI reference platform end to end testing</li>
  <li>Created production-ready Helm charts for Katib, Model Registry, KServe Web App, Notebook Controller, and Kubeflow Pipelines—enabling one-command deployment of individual components</li>
  <li>Built automated testing infrastructure with diff tools to validate Helm charts against Kustomize manifests, ensuring accuracy and catching regressions quickly</li>
  <li>Enabled incremental Kubeflow adoption, reducing deployment complexity from days to hours for organizations building production ML platforms</li>
</ul>

<p><strong>Resources:</strong></p>
<ul>
  <li>📄 <a href="https://summerofcode.withgoogle.com/programs/2025/projects/">Project Page</a></li>
  <li>🧩 <a href="https://github.com/kubeflow/community/pull/832">Kubeflow Enhancement Proposal (KEP)-831-Kubeflow-Helm-Support: Support Helm as an Alternative for Kustomize</a></li>
  <li>✍️ <a href="https://medium.com/@kunalD02/my-gsoc-journey-deploying-kubeflow-with-helm-charts-e7f9dea7b56e">Blog: My GSoC Journey: Deploying Kubeflow with Helm Charts</a></li>
</ul>

<hr />

<h3 id="project-5-jupyterlab-plugin-for-kubeflow">Project 5: JupyterLab Plugin for Kubeflow</h3>

<p><strong>Contributor:</strong> Amrit Kumar (<a href="https://github.com/Amrit27k">@Amrit27k</a>)<br />
<strong>Mentors:</strong> Eder Ignatowicz (<a href="https://github.com/ederign">@ederign</a>), Stefano Fioravanzo (<a href="https://github.com/StefanoFioravanzo">@StefanoFioravanzo</a>)</p>

<p><strong>Overview:</strong>
The project fully modernized Kubeflow Kale’s architecture, migrating the backend from KFPv1 to KFPv2 with a new Jinja2 templating system for notebook-to-pipeline conversion. The initiative also featured a complete overhaul of the JupyterLab frontend (Typescriptv5.9.2, MUIv7) and comprehensive updates to GitHub workflows, documentation, and dependencies to meet modern community standards.</p>

<p><strong>Key Outcomes:</strong></p>
<ul>
  <li>Rebuilt the Kale backend to support the modern, future-proof Kubeflow Pipelines v2 (KFPv2) architecture, moving away from the deprecated KFPv1.</li>
  <li>Implemented a new Jinja2 templating system that intelligently converts annotated Jupyter notebook cells into valid KFPv2 Python DSL scripts.</li>
  <li>Updated the JupyterLab frontend extension using current standards (Typescript v5.9.2, Jupyterlab v4, and MUI v7), resolving hundreds of legacy compatibility issues.</li>
  <li>Integrated KFPv2’s robust system for better type-safe artifact handling and automated ML Metadata registration, ensuring rich lineage tracking for pipeline steps.</li>
  <li>Standardized the project structure, updated GitHub workflows, and implemented UI test scripts to align with community standards and ensure maintainability for future contributors.</li>
</ul>

<p><strong>Resources:</strong></p>
<ul>
  <li>📄 <a href="https://github.com/kubeflow-kale/kale">Project Repo - Kubeflow Kale</a></li>
  <li>🧩 <a href="https://github.com/kubeflow-kale/kale/issues/457">Kubeflow Kale 2.0- Project Roadmap</a></li>
  <li>✍️ <a href="https://medium.com/@amritkmr4272/from-notebooks-to-pipelines-my-gsoc25-journey-modernizing-kubeflow-kale-with-kfpv2-and-e098f194208c">Blog: From Notebooks to Pipelines: My GSoC’25 Journey Modernizing Kubeflow Kale with KFPv2 and Jupyterlabv4</a></li>
</ul>

<hr />

<h3 id="project-6-spark-operator-with-kubeflow-notebooks">Project 6: Spark Operator with Kubeflow Notebooks</h3>

<p><strong>Contributor:</strong> Fellipe Resende (<a href="https://github.com/fresende">@fresende</a>)<br />
<strong>Mentors:</strong> Shekhar Rajak (<a href="https://github.com/Shekharrajak">@Shekharrajak</a>),
Luciano Resende (<a href="https://github.com/lresende">@lresende</a>),
Chaoran Yu (<a href="https://github.com/yuchaoran2011">@yuchaoran2011</a>),
Andrey Velichkevich (<a href="https://github.com/andreyvelich">@andreyvelich</a>)</p>

<p><img alt="Diagram" src="https://blog.kubeflow.org/images/2025-09-06-kubeflow-and-gsoc2025/project6.png" /></p>

<p><strong>Overview:</strong>
This project enables seamless PySpark execution within Kubeflow Notebooks by integrating the Spark Operator and Jupyter Enterprise Gateway. It allows data scientists to run distributed machine learning and big data workloads directly from their notebooks on Kubernetes, simplifying workflows and eliminating Spark infrastructure overhead, improving both usability and scalability within the Kubeflow ecosystem.</p>

<p><strong>Key Outcomes:</strong></p>

<ul>
  <li>
    <p>Extended Kubeflow Notebooks to enable seamless integration with Spark via Spark Operator leveraging Jupyter Enterprise Gateway to manage the spark application lifecycle.</p>
  </li>
  <li>
    <p>Enable data scientists and ML engineer to run distributed big-data workloads directly in Spark, from inside Kubeflow Notebooks, without manual cluster setup.</p>
  </li>
  <li>
    <p>Provided documentation and guidance for setting up, configuring, and customizing Kubeflow Notebook environments integrated with the Spark Operator, enabling users to run scalable distributed Spark workloads directly from Jupyter-based workflows.</p>
  </li>
</ul>

<p><strong>Resources:</strong></p>

<ul>
  <li>📘 <a href="https://www.kubeflow.org/docs/components/spark-operator/user-guide/notebooks-spark-operator/">Main Documentation Page</a></li>
  <li>🎥 <a href="https://youtu.be/g7tctdeitvc">Setup Demo Video</a></li>
  <li>🐞 <a href="https://www.youtube.com/watch?v=p6K6PdlkmeU">Debugging Demo Video</a></li>
  <li>📄 <a href="https://summerofcode.withgoogle.com/programs/2025/projects/zRPtxGBI">Project Page</a></li>
  <li>💻 <a href="https://github.com/kubeflow/website/pull/4141">Implementation Pull Request</a></li>
</ul>

<hr />

<h3 id="project-7-gpu-testing-for-llm-blueprints">Project 7: GPU Testing for LLM Blueprints</h3>

<p><strong>Contributor:</strong> Akash Jaiswal (<a href="https://github.com/jaiakash">@jaiakash</a>)<br />
<strong>Mentors:</strong> Andrey Velichkevich (<a href="https://github.com/andreyvelich">@andreyvelich</a>), Valentina Rodriguez Sosa(<a href="https://github.com/varodrig">@varodrig</a>)</p>

<p><img alt="Diagram" src="https://blog.kubeflow.org/images/2025-09-06-kubeflow-and-gsoc2025/project7.png" /></p>

<p><strong>Overview:</strong><br />
We had a few examples in the repository that we wanted to include in our end-to-end (E2E) tests, but all of them were CPU-based. Projects like Torchtune and Qwen 2.5, for instance, require GPU resources to run — yet our existing CI setup couldn’t validate them at all because it was entirely CPU-focused.</p>

<p>This created a major gap: whenever someone contributed a new LLM example or modified the trainer logic, we had no automated way to verify if those changes would work in a GPU environment — the same environment where these workloads are actually deployed in production.</p>

<p>The goal of this project was to add CI with GPU support directly into our CI/CD workflow.</p>

<p><strong>Key Outcomes:</strong></p>

<ul>
  <li>
    <p>Integrating GPU runners into GitHub Actions so that any pull request could automatically trigger GPU-backed E2E tests.</p>
  </li>
  <li>
    <p>Making the setup scalable and cost-efficient — instead of maintaining expensive GPU machines 24/7, we needed an on-demand system that provisions GPU resources only when a test is triggered.</p>
  </li>
</ul>

<p><strong>Resources:</strong></p>

<ul>
  <li>📄 <a href="https://summerofcode.withgoogle.com/programs/2025/projects/fwZkvPr0">Project Page</a></li>
  <li>🧩 <a href="https://github.com/kubeflow/trainer/pull/2689">Kubeflow Enhancement Proposal (KEP)</a></li>
  <li>✍️ <a href="https://my-experience-with-kubeflow-for-gsoc.hashnode.dev/gsoc-2025-with-kubeflow-scaling-gpu-testing-for-llm-blueprints">Personal Blog: Scaling GPU Testing for LLM Blueprints</a></li>
</ul>

<hr />

<h3 id="project-10-support-volcano-scheduler-in-kubeflow-trainer">Project 10: Support Volcano Scheduler in Kubeflow Trainer</h3>
<p><strong>Contributor:</strong> Xinmin Du (GitHub: <a href="https://github.com/Doris-xm">@Doris-xm</a>)<br />
<strong>Mentors:</strong> Shao Wang (<a href="https://github.com/Electronic-Waste">@Electronic-Waste</a>), Yuchen Cheng(<a href="https://github.com/rudeigerc">@rudeigerc</a>)</p>

<p><strong>Overview:</strong><br />
The project aims to integrate the <strong>Volcano scheduler</strong> into Kubeflow Trainer as a <strong>runtime plugin</strong>.
This will allow users to take advantage of advanced AI-specific scheduling features, such as Gang Scheduling and priority scheduling, supported by Volcano.</p>

<p><strong>Key Outcomes:</strong></p>
<ul>
  <li>Integrate the <strong>Volcano</strong> scheduler into Trainer as a runtime plugin to support Gang Scheduling and resource management for distributed training jobs.</li>
  <li>Enabled AI-specific features such as priority scheduling, queue-based management, and network topology–aware scheduling.</li>
</ul>

<p><strong>Resources:</strong></p>

<ul>
  <li>📄 <a href="https://summerofcode.withgoogle.com/programs/2025/projects/ZWbY1Rfj">Project Page</a></li>
  <li>🧩 <a href="https://github.com/kubeflow/trainer/pull/2672">Kubeflow Enhancement Proposal (KEP)</a></li>
</ul>

<hr />

<h3 id="project-12-empowering-kubeflow-documentation-with-llms-">Project 12: Empowering Kubeflow Documentation with LLMs 🤖</h3>
<p><strong>Contributor:</strong> Santhosh Toorpu (GitHub: <a href="https://github.com/SanthoshToorpu">@SanthoshToorpu</a>)<br />
<strong>Mentors:</strong> Francisco Javier Arceo (<a href="https://github.com/franciscojavierarceo">@franciscojavierarceo</a>), Chase Cadet (<a href="https://github.com/Chasecadet">@Chasecadet</a>)</p>

<p><strong>Overview:</strong><br />
This project introduced an intelligent documentation assistant that uses <strong>Retrieval-Augmented Generation (RAG)</strong> and <strong>KServe-hosted LLMs</strong> to enhance the Kubeflow documentation experience. The goal was to help users find relevant, accurate answers drawn from Kubeflow docs, GitHub issues, and community discussions — all through a conversational interface on the Kubeflow website.</p>

<p>The system leverages <strong>Kubeflow Pipelines</strong> to automate documentation ingestion and indexing, <strong>Milvus</strong> for semantic vector search, and <strong>FastAPI with WebSockets</strong> for real-time interactions. Built on Kubernetes, the architecture follows Kubeflow’s MLOps principles end-to-end — from automated retraining and indexing to monitored LLM inference served via KServe.</p>

<p><strong>Key Outcomes:</strong></p>
<ul>
  <li>Designed and deployed an <strong>LLM-powered Documentation Assistant</strong> using Kubeflow-native tools (KFP, KServe, Feast, Milvus).</li>
  <li>Implemented <strong>automated documentation indexing pipelines</strong> triggered by GitHub Actions to keep vector embeddings up-to-date.</li>
  <li>Developed an <strong>interactive chat interface</strong> integrated into the Kubeflow website for natural-language documentation search.</li>
  <li>Introduced a <strong>RAG agentic workflow</strong> with tool-calling to decide when to retrieve external documentation or use model knowledge.</li>
  <li>Implemented <strong>RBAC-based access control</strong> for pipelines and KServe endpoints to align with Kubeflow’s multi-user isolation standards.</li>
  <li>Developed a <strong>feedback loop system</strong> (“👍 / 👎”) to improve the model’s performance and documentation quality.</li>
  <li>Delivered a functional prototype hosted on Kubernetes, showcasing real-time semantic search across Kubeflow repositories.</li>
</ul>

<p><strong>Resources:</strong></p>
<ul>
  <li>📄 <a href="https://summerofcode.withgoogle.com/programs/2025/projects/a9JPxfEh">Project Page</a></li>
  <li>🧠 <a href="https://github.com/kubeflow/docs-agent">Demo Repo</a></li>
  <li>✍️ <a href="https://medium.com/@toorpusanthosh/empowering-kubeflow-documentation-with-llms-my-gsoc-journey-58eb946ba2af">Blog Post: Empowering Kubeflow Documentation with LLMs</a> <!-- Add blog link here when published --></li>
</ul>

<hr />

<h2 id="-wrapping-up">🎉 Wrapping Up</h2>

<p>We are proud of what our GSoC 2025 contributors achieved and the impact they have made on the Kubeflow ecosystem. Their work not only strengthens existing components but also lays the foundation for future innovation in MLOps and AI infrastructure.</p>

<p>A huge <strong>thank you</strong> 🙏 to all contributors, mentors, and community members who made this program a success.</p>

<hr />

<h2 id="-want-to-get-involved">👩‍💻 Want to Get Involved?</h2>

<p>The Kubeflow community is open to contributors of all backgrounds and skill levels. Whether you are passionate about ML infrastructure, frontend, DevOps, or documentation — there’s a place for you here.</p>

<ul>
  <li>💻 Visit our <a href="https://www.kubeflow.org/docs/about/community/">website</a> and <a href="https://github.com/kubeflow">GitHub</a></li>
  <li>💬 Join our <a href="https://www.kubeflow.org/docs/about/community/">Slack</a></li>
  <li>🗓️ Attend the <a href="https://www.kubeflow.org/docs/about/community/#kubeflow-community-call">community calls</a></li>
  <li>📩 Subscribe to the <a href="https://groups.google.com/g/kubeflow-discuss">kubeflow-discuss</a> mailing list</li>
</ul>

<p>Let’s continue building the future of MLOps together 🚀</p>
