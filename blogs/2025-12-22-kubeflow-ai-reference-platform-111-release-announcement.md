---
title: "Kubeflow AI Reference Platform 1.11 Release Announcement"
url: "https://blog.kubeflow.org/kubeflow-1.11-release/"
date: "2025-12-22T00:00:00-06:00"
author: "Kubeflow 1.11 Release Team"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<p>Kubeflow AI Reference Platform 1.11 delivers substantial platform improvements focused on scalability, security, and operational efficiency. The release reduces per namespace overhead, strengthens multi-tenant defaults, and improves overall reliability for running Kubeflow at scale on Kubernetes.</p>

<h2 id="highlight-features">Highlight features</h2>

<ul>
  <li>Trainer v2.1.0 with unified TrainJob API, Python-first workflows, and built-in LLM fine-tuning support</li>
  <li>Multi-tenant S3 storage with per-namespace credentials, with SeaweedFS replacing MinIO as the default backend</li>
  <li>Massive scalability improvements enabling Kubeflow deployments to scale to 1,000+ users, profiles, and namespaces</li>
  <li>Zero pod overhead by default for namespaces and profiles, significantly reducing baseline resource consumption</li>
  <li>Optimized Istio service mesh configuration to dramatically reduce sidecar memory usage and network traffic in large clusters</li>
  <li>Stronger security defaults with Pod Security Standards (restricted for system namespaces, baseline for user namespaces)</li>
  <li>Improved authentication and exposure patterns for KServe inference services, with automated tests and documentation</li>
  <li>Expanded Helm chart support (experimental) to improve modularity and deployment flexibility</li>
  <li>Updates across core components, including Kubeflow Pipelines, Katib, KServe, Model Registry, Istio, and Spark Operator</li>
</ul>

<h2 id="kubeflow-platform-manifests--security">Kubeflow Platform (Manifests &amp; Security)</h2>

<p>The Kubeflow Platform Working Group focuses on simplifying Kubeflow installation, operations, and security. See details below.</p>

<h3 id="manifests">Manifests:</h3>

<ul>
  <li><a href="https://github.com/kubeflow/manifests/blob/master/README.md">Documentation updates</a> that make it easier to install,
extend and upgrade Kubeflow</li>
  <li>For more details and future plans please check <a href="https://github.com/kubeflow/manifests/issues/3038">1.12.0</a> roadmap.</li>
</ul>

<table>
  <thead>
    <tr>
      <th style="text-align: center;">Notebooks</th>
      <th style="text-align: center;">Dashboard</th>
      <th style="text-align: center;">Pipelines</th>
      <th style="text-align: center;">Katib</th>
      <th style="text-align: center;">Trainer</th>
      <th style="text-align: center;">KServe</th>
      <th style="text-align: center;">Model Registry</th>
      <th style="text-align: center;">Spark</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;"><a href="https://github.com/kubeflow/kubeflow/issues/7459">1.10</a></td>
      <td style="text-align: center;"><a href="https://github.com/kubeflow/kubeflow/releases/tag/v1.10.0">1.10</a></td>
      <td style="text-align: center;"><a href="https://github.com/kubeflow/pipelines/releases/tag/2.15.2">2.15.2</a></td>
      <td style="text-align: center;"><a href="https://github.com/kubeflow/katib/releases/tag/v0.19.0">0.19.0</a></td>
      <td style="text-align: center;"><a href="https://github.com/kubeflow/trainer/releases/tag/v2.1.0">2.1.0</a></td>
      <td style="text-align: center;"><a href="https://github.com/kserve/kserve/releases/tag/v0.15.2">0.15.2</a></td>
      <td style="text-align: center;"><a href="https://github.com/kubeflow/model-registry/releases/tag/v0.3.4">0.3.4</a></td>
      <td style="text-align: center;"><a href="https://github.com/kubeflow/spark-operator/releases/tag/v2.4.0">2.4.0</a></td>
    </tr>
  </tbody>
</table>

<table>
  <thead>
    <tr>
      <th style="text-align: center;">Kubernetes</th>
      <th style="text-align: center;">Kind</th>
      <th style="text-align: center;">Kustomize</th>
      <th style="text-align: center;">Cert Manager</th>
      <th style="text-align: center;">Knative</th>
      <th style="text-align: center;">Istio</th>
      <th style="text-align: center;">Dex</th>
      <th style="text-align: center;">OAuth2-proxy</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;">1.33+</td>
      <td style="text-align: center;">0.30.0</td>
      <td style="text-align: center;">5.7.1</td>
      <td style="text-align: center;">1.16.1</td>
      <td style="text-align: center;">1.20</td>
      <td style="text-align: center;">1.28</td>
      <td style="text-align: center;">2.43</td>
      <td style="text-align: center;">7.10</td>
    </tr>
  </tbody>
</table>

<h3 id="security">Security:</h3>

<ul>
  <li><strong>Pod Security Standards enforced by default</strong>:
    <ul>
      <li><code class="language-plaintext highlighter-rouge">restricted</code> for all Kubeflow system namespaces<br />
(<a href="https://github.com/kubeflow/manifests/pull/3190">#3190</a>, <a href="https://github.com/kubeflow/manifests/pull/3050">#3050</a>)</li>
      <li><code class="language-plaintext highlighter-rouge">baseline</code> for user namespaces<br />
(<a href="https://github.com/kubeflow/manifests/pull/3204">#3204</a>, <a href="https://github.com/kubeflow/manifests/pull/3220">#3220</a>)</li>
    </ul>
  </li>
  <li><strong>Network policies enabled by default</strong> for critical system namespaces<br />
(<code class="language-plaintext highlighter-rouge">knative-serving</code>, <code class="language-plaintext highlighter-rouge">oauth2-proxy</code>, <code class="language-plaintext highlighter-rouge">cert-manager</code>, <code class="language-plaintext highlighter-rouge">istio-system</code>, <code class="language-plaintext highlighter-rouge">auth</code>)<br />
(<a href="https://github.com/kubeflow/manifests/pull/3228">#3228</a>)</li>
  <li><strong>Improved multi-tenant isolation for object storage</strong>, with per-namespace S3 credentials<br />
(<a href="https://github.com/kubeflow/manifests/pull/3240">#3240</a>)</li>
  <li><strong>Authentication enforcement for KServe inference services</strong><br />
(<a href="https://github.com/kubeflow/manifests/pull/3180">#3180</a>)</li>
</ul>

<p>Trivy CVE scans December 15 2025:</p>

<table>
  <thead>
    <tr>
      <th style="text-align: center;">Working Group</th>
      <th style="text-align: center;">Images</th>
      <th style="text-align: center;">Critical CVE</th>
      <th style="text-align: center;">High CVE</th>
      <th style="text-align: center;">Medium CVE</th>
      <th style="text-align: center;">Low CVE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align: center;">Katib</td>
      <td style="text-align: center;">18</td>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">35</td>
      <td style="text-align: center;">158</td>
      <td style="text-align: center;">562</td>
    </tr>
    <tr>
      <td style="text-align: center;">Pipelines</td>
      <td style="text-align: center;">15</td>
      <td style="text-align: center;">12</td>
      <td style="text-align: center;">432</td>
      <td style="text-align: center;">1051</td>
      <td style="text-align: center;">1558</td>
    </tr>
    <tr>
      <td style="text-align: center;">Workbenches(Notebooks)</td>
      <td style="text-align: center;">12</td>
      <td style="text-align: center;">39</td>
      <td style="text-align: center;">312</td>
      <td style="text-align: center;">525</td>
      <td style="text-align: center;">267</td>
    </tr>
    <tr>
      <td style="text-align: center;">Kserve</td>
      <td style="text-align: center;">16</td>
      <td style="text-align: center;">35</td>
      <td style="text-align: center;">535</td>
      <td style="text-align: center;">11929</td>
      <td style="text-align: center;">1745</td>
    </tr>
    <tr>
      <td style="text-align: center;">Manifests</td>
      <td style="text-align: center;">15</td>
      <td style="text-align: center;">6</td>
      <td style="text-align: center;">105</td>
      <td style="text-align: center;">256</td>
      <td style="text-align: center;">55</td>
    </tr>
    <tr>
      <td style="text-align: center;">Trainer</td>
      <td style="text-align: center;">9</td>
      <td style="text-align: center;">4</td>
      <td style="text-align: center;">157</td>
      <td style="text-align: center;">9012</td>
      <td style="text-align: center;">728</td>
    </tr>
    <tr>
      <td style="text-align: center;">Model Registry</td>
      <td style="text-align: center;">3</td>
      <td style="text-align: center;">3</td>
      <td style="text-align: center;">75</td>
      <td style="text-align: center;">132</td>
      <td style="text-align: center;">36</td>
    </tr>
    <tr>
      <td style="text-align: center;">Spark</td>
      <td style="text-align: center;">1</td>
      <td style="text-align: center;">4</td>
      <td style="text-align: center;">22</td>
      <td style="text-align: center;">1688</td>
      <td style="text-align: center;">151</td>
    </tr>
    <tr>
      <td style="text-align: center;">All Images</td>
      <td style="text-align: center;">89</td>
      <td style="text-align: center;">104</td>
      <td style="text-align: center;">1673</td>
      <td style="text-align: center;">24751</td>
      <td style="text-align: center;">5102</td>
    </tr>
  </tbody>
</table>

<h2 id="pipelines">Pipelines</h2>

<p>This release of KFP introduces several notable changes that users should consider prior to upgrading. Comprehensive upgrade and documentation notes will follow shortly. In the interim, please note the following key modifications</p>

<h3 id="default-object-store-update">Default object store update</h3>

<p>Kubeflow Pipelines now defaults to SeaweedFS for the object store deployment, replacing the previous default of MinIO.
MinIO remains fully supported, as does any S3-compatible object storage backend, only the default deployment configuration has changed.</p>

<p>Existing MinIO manifests are still available for users who wish to continue using MinIO, though these legacy manifests may be removed in future releases. Users with existing data are advised to back up and restore as needed when switching object store backends.</p>

<h3 id="database-backend-upgrade">Database backend upgrade</h3>

<p>This release includes a major upgrade to the Gorm database backend, which introduces an automated database index migration for users upgrading from versions prior to 2.15.0.
Because this migration does not support rollback, it is strongly recommended that production databases be backed up before performing the upgrade.</p>

<h2 id="model-registry">Model Registry</h2>

<p>Model Registry continues to mature with new capabilities for model discovery, governance, and deeper integration with the Kubeflow ecosystem.</p>

<h3 id="model-registry-ui">Model Registry UI</h3>

<p>The user-friendly web interface for centralized model metadata, version tracking, and artifact management now supports filtering, sorting, archiving, custom metadata, and metadata editing making it easier for teams to organize and govern their model lifecycle.</p>

<h3 id="model-catalog">Model Catalog</h3>

<p>A new Model Catalog feature enables model discovery and sharing with governance controls.
A <a href="https://github.com/kubeflow/community/blob/master/proposals/907-model-registry-renaming/README.md#model-catalog-cluster-scoped-company-scoped">Model Catalog</a> is a pattern where an organisation can define their validated and approved models, enabling discovery and sharing across teams, while at the same time ensuring model governance and compliance.
Admin can define a number of catalog sources, filtering and enable model visibility, including Hugging Face.
Teams can discover and use approved models from the organisation’s catalog.
The catalog UI and backend are under active development.</p>

<h3 id="kserve-integration">KServe Integration</h3>

<ul>
  <li><strong>Custom Storage Initializer (CSI)</strong>  Enables model download and deployment using model metadata directly from the Registry.</li>
  <li><strong>Reconciliation loop</strong>  A deployable Kubernetes controller which observes KServe InferenceServices to automatically populate Model Registry logical-model records, keeping registry audit records of live deployments.</li>
</ul>

<h3 id="storage-integrations">Storage Integrations</h3>

<ul>
  <li><strong>Python client workflows</strong>  Data scientists can leverage convenience functions in the Python client to <a href="https://model-registry.readthedocs.io/en/latest/#uploading-local-models-to-external-storage-and-registering-them">package, store, and register models and their metadata</a> in a single playbook.</li>
  <li><strong>Async Upload Job</strong>  A Kubernetes Job for transferring and packaging models (including KServe ModelCar OCI Image format), simplifying model storage operations in production environments, leveraging scaling and orchestration capabilities of Kubernetes without additional dependencies.</li>
</ul>

<h3 id="additional-improvements">Additional Improvements</h3>

<ul>
  <li>Removal of the legacy Google MLMD dependency.</li>
  <li>PostgreSQL support alongside MySQL.</li>
  <li>Multi-architecture container builds (amd64/arm64).</li>
  <li>SBOM generation for container builds and OpenSSF Scorecard CI integration.</li>
</ul>

<h2 id="training-operator-trainer--katib">Training Operator (Trainer) &amp; Katib</h2>

<p>Kubeflow 1.11 includes Trainer v2.1.0, a major architectural evolution that simplifies distributed training on Kubernetes with a unified API, Python-first workflows, and enhanced LLM fine-tuning capabilities.</p>

<h3 id="new-api-architecture">New API Architecture</h3>

<p>Kubeflow Trainer v2 introduces <strong>TrainJob</strong>  a unified training job API that replaces framework-specific CRDs (PyTorchJob, TFJob, etc.). Infrastructure configuration is now separated into <strong>TrainingRuntime</strong> and <strong>ClusterTrainingRuntime</strong> resources, creating a clean boundary between platform engineering (runtime setup) and data science (job submission).</p>

<h3 id="python-first-experience">Python-First Experience</h3>

<ul>
  <li><strong>No YAML required</strong>  Install with <code class="language-plaintext highlighter-rouge">pip install kubeflow</code> and submit jobs directly from Python notebooks or scripts.</li>
  <li><strong>Local execution mode</strong>  Develop and test training code locally without a Kubernetes cluster before scaling to production.</li>
  <li><strong>Helm Charts</strong>  Deploy with <code class="language-plaintext highlighter-rouge">helm install kubeflow-trainer oci://ghcr.io/kubeflow/charts/kubeflow-trainer --version 2.1.0</code>.</li>
</ul>

<h3 id="llm-fine-tuning">LLM Fine-Tuning</h3>

<p>Built-in support for large language model fine-tuning workflows:</p>
<ul>
  <li>TorchTune trainer with pre-configured runtimes for Llama 3.2, Qwen 2.5, and more.</li>
  <li>LoRA, QLoRA, and DoRA for parameter-efficient fine-tuning.</li>
  <li>Dataset and model initializers for HuggingFace and S3 storage.</li>
</ul>

<h3 id="distributed-ai-data-cache">Distributed AI Data Cache</h3>

<p>Optional in-memory cache cluster (powered by <a href="https://arrow.apache.org/">Apache Arrow</a> and <a href="https://datafusion.apache.org/">Apache DataFusion</a> ) streams datasets directly to GPU nodes with zero-copy transfers, maximizing GPU utilization and minimizing I/O wait times for large-scale training workloads. More details can be found <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/data-cache/">here</a>.</p>

<h3 id="scheduler-integrations">Scheduler Integrations</h3>

<ul>
  <li><strong>Kueue</strong>  Topology-aware scheduling and multi-cluster job dispatching for TrainJobs, enabling optimal placement for distributed training across node groups.</li>
  <li><strong>Volcano</strong>  Gang-scheduling support with PodGroup integration.</li>
  <li><strong>MPI</strong>  First-class support for MPI-based distributed training workloads on Kubernetes.</li>
</ul>

<h3 id="katib">Katib</h3>

<p>Katib hyperparameter tuning remains compatible with Trainer v2, allowing users to optimize model hyperparameters alongside the new training workflow.</p>

<p>A major addition is the integration with Kubeflow SDK (<a href="https://github.com/kubeflow/sdk/tree/main/docs/proposals/46-hyperparameter-optimization">KEP-46</a>, <a href="https://github.com/kubeflow/sdk/pull/124">PR #124</a>). The new <code class="language-plaintext highlighter-rouge">OptimizerClient</code> allows users to define and run hyperparameter experiments directly from Python notebooks without writing YAML. You can configure search spaces, objectives, and algorithms using <code class="language-plaintext highlighter-rouge">OptimizerClient().optimize()</code>. Each trial runs as a TrainJob with different hyperparameter values, and training code can report metrics using simple Python functions. The client includes standard methods for managing jobs: <code class="language-plaintext highlighter-rouge">create_job()</code>, <code class="language-plaintext highlighter-rouge">get_job()</code>, <code class="language-plaintext highlighter-rouge">list_jobs()</code>, and <code class="language-plaintext highlighter-rouge">delete_job()</code>.</p>

<h2 id="spark-operator">Spark Operator</h2>

<p>The Spark Operator has received broad improvements in Kubeflow 1.11, spanning Spark version support, workload management, scheduling, and operational simplicity.</p>

<h3 id="broader-spark-support">Broader Spark Support</h3>

<p>The operator now supports Apache Spark 4 and introduces Spark Connect, enabling modern client–server Spark interactions. This allows users to connect to Spark sessions remotely and improves compatibility with the evolving Spark ecosystem.</p>

<h3 id="workload-management--scheduling">Workload Management &amp; Scheduling</h3>

<ul>
  <li><strong>Suspend / Resume SparkApplications</strong>  Users can now suspend and resume jobs, giving greater control over workload lifecycle.</li>
  <li><strong>Kueue integration</strong>  Integration with <a href="https://kueue.sigs.k8s.io/">Kueue</a> enables queue-based workload management and fair sharing of cluster resources across teams.</li>
  <li><strong>Enhanced dynamic allocation</strong>  Improved shuffle tracking and dynamic allocation controls for more efficient resource usage.</li>
</ul>

<h3 id="operations--security">Operations &amp; Security</h3>

<ul>
  <li><strong>Automatic CRD upgrades</strong>  Helm hooks now handle CRD upgrades automatically, reducing manual steps during upgrades.</li>
  <li><strong>Deprecation of sparkctl</strong>  Legacy <code class="language-plaintext highlighter-rouge">sparkctl</code> has been deprecated in favor of kubectl-native workflows.</li>
  <li><strong>Flexible Ingress &amp; cert-manager support</strong>  More configurable Ingress (TLS, annotations, URL patterns) and simplified certificate handling via cert-manager.</li>
</ul>

<h3 id="observability">Observability</h3>

<ul>
  <li><strong>Structured logging</strong>  Configurable JSON and console log output formats.</li>
  <li><strong>Better validation</strong>  Stricter validation of SparkApplication names and specs, catching misconfigurations earlier.</li>
</ul>

<h2 id="kserve">KServe</h2>

<p>KServe in Kubeflow 1.11 delivers major improvements across model serving, inference capabilities, and operational maturity.</p>

<h3 id="multi-node-inference">Multi-Node Inference</h3>

<p>KServe now supports multi-node inference, enabling large models to be distributed across multiple nodes using Ray-based serving runtimes. This is critical for deploying very large language models that exceed single-node GPU capacity.</p>

<h3 id="model-cache-improvements">Model Cache Improvements</h3>

<p>The Model Cache feature, introduced in v0.14, has been significantly hardened. Fixes include correct URI matching, protection against cache mismatches, support for multiple node groups, and PVC/PV retention after InferenceService deletion making model caching more reliable for production use.</p>

<h3 id="keda-autoscaling-integration">KEDA Autoscaling Integration</h3>

<p>KServe introduces integration with <a href="https://keda.sh/">KEDA</a> for event-driven autoscaling, including an external scaler implementation. This gives users more flexible scaling options beyond the built-in Knative and HPA-based autoscalers.</p>

<h3 id="gateway-api-support">Gateway API Support</h3>

<p>Raw deployment mode now supports the Kubernetes Gateway API, providing a modern, standardized alternative to Ingress for routing inference traffic.</p>

<h3 id="vllm--hugging-face-runtime-updates">vLLM &amp; Hugging Face Runtime Updates</h3>

<ul>
  <li>Upgraded vLLM to v0.8.1+ with support for reasoning models, tool calling, embeddings, reranking, and Llama 4 / Qwen 3.</li>
  <li>vLLM V1 engine support and CPU inference via Intel Extension for PyTorch.</li>
  <li>LMCache integration with vLLM for improved KV cache reuse.</li>
  <li>Hugging Face runtime updates include 4-bit quantization support (bitsandbytes), speculative decoding, and deprecation of OpenVINO support.</li>
</ul>

<h3 id="inference-graph-enhancements">Inference Graph Enhancements</h3>

<ul>
  <li>InferenceGraphs now support pod spec fields (affinity, tolerations, resources) and well-known labels.</li>
  <li>Improved Istio mesh compatibility and fixed response codes for conditional routing steps.</li>
</ul>

<h3 id="operational--security-improvements">Operational &amp; Security Improvements</h3>

<ul>
  <li>ModelCar (OCI-based model loading) enabled by default.</li>
  <li>Collocation of transformer and predictor containers in a single pod.</li>
  <li>Stop-and-resume model serving via annotations (serverless mode).</li>
  <li>Configurable label and annotation propagation to serving pods.</li>
  <li>SBOM generation and third-party license inclusion for all images.</li>
  <li>Multiple CVE fixes including <code class="language-plaintext highlighter-rouge">CVE-2025-43859</code> and <code class="language-plaintext highlighter-rouge">CVE-2025-24357</code>.</li>
</ul>

<h2 id="kubeflow-sdk">Kubeflow SDK</h2>

<p>Kubeflow 1.11 is the first AI Reference Platform release where users can simply <code class="language-plaintext highlighter-rouge">pip install kubeflow</code> to start working with AI workloads, no Kubernetes expertise required. The <a href="https://sdk.kubeflow.org/en/latest/">Kubeflow SDK</a> provides a unified Python interface to train models, run hyperparameter tuning, and manage model artifacts across the Kubeflow ecosystem. It also enables local development without a Kubernetes cluster, so users can iterate on their training code locally before scaling to production. For documentation and examples, visit <a href="https://sdk.kubeflow.org/en/latest/">sdk.kubeflow.org</a>.</p>

<h2 id="dashboard-and-notebooks">Dashboard and Notebooks</h2>

<p>The Kubeflow Central Dashboard and Notebooks remain at version 1.10 in this release, providing stable and reliable experiences. Stay tuned for interesting updates in upcoming Kubeflow AI Reference Platform releases.</p>

<h2 id="how-to-get-started-with-111">How to get started with 1.11</h2>

<p>Visit the Kubeflow AI Reference Platform 1.11 <a href="https://github.com/kubeflow/manifests/releases">release page</a> or head over to the Getting Started and Support pages.</p>

<h2 id="join-the-community">Join the Community</h2>

<p>We would like to thank everyone who contributed to Kubeflow 1.11, and especially Valentina Rodriguez Sosa for her work as the v1.11 Release Manager. We also extend our thanks to the entire release team and the working group leads, who continuously and generously dedicate their time and expertise to Kubeflow.</p>

<p>Release team members : Valentina Rodriguez Sosa, Anya Kramar, Tarek Abouzeid, Andy Stoneberg, Humair Khan, Matteo Mortari, Adysen Rothman, Jon Burdo, Milos Grubjesic, Vraj Bhatt, Dhanisha Phadate, Alok Dangre</p>

<p>Working Group leads : Andrey Velichkevich, Julius von Kohout,  Mathew Wicks, Matteo Mortari</p>

<p>Kubeflow Steering Committee : Andrey Velichkevich, Julius von Kohout, Yuan Tang, Johnu George, Francisco Javier Araceo</p>

<p>You can find more details about Kubeflow distributions
<a href="https://www.kubeflow.org/docs/started/installing-kubeflow/#packaged-distributions">here</a>.</p>

<h2 id="want-to-help">Want to help?</h2>

<p>The Kubeflow community Working Groups hold open meetings and are always looking for more volunteers and users to unlock
the potential of machine learning. If you’re interested in becoming a Kubeflow contributor, please feel free to check
out the resources below. We look forward to working with you!</p>

<ul>
  <li>Visit our <a href="https://www.kubeflow.org/docs/about/community/">Kubeflow website</a> or Kubeflow GitHub Page.</li>
  <li>Join the <a href="https://www.kubeflow.org/docs/about/community/">Kubeflow Slack channel</a>.</li>
  <li>Join the <a href="https://groups.google.com/g/kubeflow-discuss">kubeflow-discuss</a> mailing list.</li>
  <li>Attend our weekly <a href="https://www.kubeflow.org/docs/about/community/#kubeflow-community-call">community meeting</a>.</li>
</ul>
