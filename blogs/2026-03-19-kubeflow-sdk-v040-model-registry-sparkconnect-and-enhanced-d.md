---
title: "Kubeflow SDK v0.4.0: Model Registry, SparkConnect, and Enhanced Developer Experience"
url: "https://blog.kubeflow.org/kubeflow-sdk-0.4.0-release/"
date: "2026-03-19T00:00:00-05:00"
author: "Kubeflow SDK Team"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<blockquote>
  <p><strong>Explore the full documentation at <a href="https://sdk.kubeflow.org">sdk.kubeflow.org</a></strong></p>
</blockquote>

<p>With KubeCon just around the corner, we are pleased to announce the release of Kubeflow SDK v0.4.0. This release continues the work toward providing a unified, Pythonic interface for all AI workloads on Kubernetes.</p>

<p>The v0.4.0 release focuses on bridging the gap between data engineering, model management, and production-ready ML pipelines. The Kubeflow SDK now covers most of the MLOps lifecycle – from data processing and hyperparameter optimization to model training and registration:</p>

<p><img alt="Kubeflow SDK Diagram" src="https://blog.kubeflow.org/images/2026-03-19-kubeflow-sdk-0.4.0-release/kubeflow-sdk.png" /></p>

<p>Highlights in Kubeflow SDK v0.4.0 include:</p>

<ul>
  <li><a href="https://sdk.kubeflow.org/en/latest/hub/index.html">Model Registry Client</a> for managing model artifacts, versions, and metadata directly from the SDK.</li>
  <li><a href="https://sdk.kubeflow.org/en/latest/spark/index.html">SparkClient API</a> with SparkConnect support for interactive data processing</li>
  <li><a href="https://blog.kubeflow.org/kubeflow-sdk-0.4.0-release/#better-isolation-with-namespaced-trainingruntimes">Namespaced TrainingRuntimes</a> for improved isolation and multi-tenant platform management</li>
  <li><a href="https://blog.kubeflow.org/kubeflow-sdk-0.4.0-release/#furthering-parity-between-local-and-remote-execution">Dataset and Model Initializers</a> enabling better parity between local and Kubernetes execution</li>
  <li><a href="https://blog.kubeflow.org/kubeflow-sdk-0.4.0-release/#a-new-home-for-documentation">A new Kubeflow SDK documentation website</a> with examples, and API reference</li>
  <li><a href="https://blog.kubeflow.org/kubeflow-sdk-0.4.0-release/#required-upgrading-to-python-310">Minimum Python version updated</a> to Python 3.10 for improved security, typing, and runtime performance</li>
</ul>

<h2 id="unified-model-management-the-model-registry-client">Unified Model Management: The Model Registry Client</h2>

<p>Managing model artifacts, versions, and metadata across experiments has historically required stitching together multiple tools outside of your training code. In v0.4.0, the SDK introduces <code class="language-plaintext highlighter-rouge">ModelRegistryClient</code> – a Pythonic interface to the Kubeflow Model Registry, available under the new <code class="language-plaintext highlighter-rouge">kubeflow.hub</code> submodule.</p>

<p>The client exposes a minimal, curated API: register models, retrieve them by name and version, update their metadata, and iterate over what’s in your registry – all without leaving the SDK. It integrates directly with the Model Registry server and supports token auth and custom CA configuration for production clusters. To install the Model Registry server, see the <a href="https://www.kubeflow.org/docs/components/model-registry/installation/">installation guide</a>.</p>

<p>Install the hub extra to get started:</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>pip <span class="nb">install</span> <span class="s1">'kubeflow[hub]'</span>
</code></pre></div></div>

<h3 id="usage-example">Usage Example</h3>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.hub</span> <span class="kn">import</span> <span class="n">ModelRegistryClient</span>

<span class="n">client</span> <span class="o">=</span> <span class="n">ModelRegistryClient</span><span class="p">(</span>
    <span class="s">"https://model-registry.kubeflow.svc.cluster.local"</span><span class="p">,</span>
    <span class="n">author</span><span class="o">=</span><span class="s">"Your Name"</span><span class="p">,</span>
<span class="p">)</span>

<span class="c1"># Register a model
</span><span class="n">model</span> <span class="o">=</span> <span class="n">client</span><span class="p">.</span><span class="n">register_model</span><span class="p">(</span>
    <span class="n">name</span><span class="o">=</span><span class="s">"my-model"</span><span class="p">,</span>
    <span class="n">uri</span><span class="o">=</span><span class="s">"s3://bucket/path/to/model"</span><span class="p">,</span>
    <span class="n">version</span><span class="o">=</span><span class="s">"1.0.0"</span><span class="p">,</span>
    <span class="n">model_format_name</span><span class="o">=</span><span class="s">"pytorch"</span><span class="p">,</span>
<span class="p">)</span>

<span class="c1"># List all models
</span><span class="k">for</span> <span class="n">model</span> <span class="ow">in</span> <span class="n">client</span><span class="p">.</span><span class="n">list_models</span><span class="p">():</span>
    <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"Model: </span><span class="si">{</span><span class="n">model</span><span class="p">.</span><span class="n">name</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>

<span class="c1"># Get a specific version and artifact
</span><span class="n">version</span> <span class="o">=</span> <span class="n">client</span><span class="p">.</span><span class="n">get_model_version</span><span class="p">(</span><span class="s">"my-model"</span><span class="p">,</span> <span class="s">"1.0.0"</span><span class="p">)</span>
<span class="n">artifact</span> <span class="o">=</span> <span class="n">client</span><span class="p">.</span><span class="n">get_model_artifact</span><span class="p">(</span><span class="s">"my-model"</span><span class="p">,</span> <span class="s">"1.0.0"</span><span class="p">)</span>
<span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"Model URI: </span><span class="si">{</span><span class="n">artifact</span><span class="p">.</span><span class="n">uri</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>
</code></pre></div></div>

<blockquote>
  <p><strong>Note:</strong> <code class="language-plaintext highlighter-rouge">list_models()</code> and <code class="language-plaintext highlighter-rouge">list_model_versions()</code> return lazy iterators backed by pagination, so only the data you consume results in API calls – making it efficient to work with large registries.</p>
</blockquote>

<h2 id="distributed-ai-data-at-scale-sparkclient--sparkconnect">Distributed AI Data at Scale: SparkClient &amp; SparkConnect</h2>

<p>Data is a fundamental piece to every AI workload, and Apache Spark has become a cornerstone technology for large-scale data processing. However, deploying and managing Spark workloads on Kubernetes has traditionally required users to work directly with Kubernetes manifests and YAML configurations – a process that can be operationally complex. In v0.4.0, the SDK introduces <code class="language-plaintext highlighter-rouge">SparkClient</code> – a high-level, Pythonic API that eliminates this complexity, allowing data engineers and ML practitioners to manage interactive and batch Spark workloads on Kubernetes without writing a single line of YAML. Backed by the Kubeflow Spark Operator (<a href="https://github.com/kubeflow/sdk/blob/main/docs/proposals/107-spark-client/README.md">KEP-107</a>), the initial version of SparkClient introduces support for interactive sessions through the SparkConnect custom resource. In future releases of the Kubeflow SDK, we will expand this support to include batch workloads as well.</p>

<p><code class="language-plaintext highlighter-rouge">SparkClient</code> supports two operational modes. In <strong>create mode</strong>, the SDK provisions a new SparkConnect interactive session on Kubernetes for you – handling CRD creation, pod scheduling, networking, and cleanup automatically. In <strong>connect mode</strong>, you point it at an existing Spark Connect server, useful for shared clusters or cross-namespace access. Either way, you get back a standard <code class="language-plaintext highlighter-rouge">SparkSession</code> and can write the same PySpark code you already know.</p>

<p>Install Kubeflow Spark support:</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>pip <span class="nb">install</span> <span class="s1">'kubeflow[spark]'</span>
</code></pre></div></div>

<p>To install the Spark Operator, see the <a href="https://www.kubeflow.org/docs/components/spark-operator/getting-started/">installation guide</a>.</p>

<h3 id="usage-example-1">Usage Example</h3>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.spark</span> <span class="kn">import</span> <span class="n">SparkClient</span><span class="p">,</span> <span class="n">Name</span>
<span class="kn">from</span> <span class="nn">kubeflow.common.types</span> <span class="kn">import</span> <span class="n">KubernetesBackendConfig</span>

<span class="n">client</span> <span class="o">=</span> <span class="n">SparkClient</span><span class="p">(</span>
    <span class="n">backend_config</span><span class="o">=</span><span class="n">KubernetesBackendConfig</span><span class="p">(</span><span class="n">namespace</span><span class="o">=</span><span class="s">"spark-test"</span><span class="p">)</span>
<span class="p">)</span>

<span class="c1"># Level 1: Minimal - use all defaults
</span><span class="n">spark</span> <span class="o">=</span> <span class="n">client</span><span class="p">.</span><span class="n">connect</span><span class="p">(</span><span class="n">options</span><span class="o">=</span><span class="p">[</span><span class="n">Name</span><span class="p">(</span><span class="s">"my-session"</span><span class="p">)])</span>
<span class="n">df</span> <span class="o">=</span> <span class="n">spark</span><span class="p">.</span><span class="nb">range</span><span class="p">(</span><span class="mi">5</span><span class="p">)</span>
<span class="n">df</span><span class="p">.</span><span class="n">show</span><span class="p">()</span>
<span class="n">client</span><span class="p">.</span><span class="n">delete_session</span><span class="p">(</span><span class="s">"my-session"</span><span class="p">)</span>

<span class="c1"># Level 2: Simple -- configure executors and resources
</span><span class="n">spark</span> <span class="o">=</span> <span class="n">client</span><span class="p">.</span><span class="n">connect</span><span class="p">(</span>
    <span class="n">num_executors</span><span class="o">=</span><span class="mi">5</span><span class="p">,</span>
    <span class="n">resources_per_executor</span><span class="o">=</span><span class="p">{</span><span class="s">"cpu"</span><span class="p">:</span> <span class="s">"5"</span><span class="p">,</span> <span class="s">"memory"</span><span class="p">:</span> <span class="s">"1Gi"</span><span class="p">},</span>
    <span class="n">spark_conf</span><span class="o">=</span><span class="p">{</span><span class="s">"spark.sql.adaptive.enabled"</span><span class="p">:</span> <span class="s">"true"</span><span class="p">},</span>
    <span class="n">options</span><span class="o">=</span><span class="p">[</span><span class="n">Name</span><span class="p">(</span><span class="s">"my-session-2"</span><span class="p">)],</span>
<span class="p">)</span>
<span class="n">df</span> <span class="o">=</span> <span class="n">spark</span><span class="p">.</span><span class="nb">range</span><span class="p">(</span><span class="mi">5</span><span class="p">)</span>
<span class="n">df</span><span class="p">.</span><span class="n">show</span><span class="p">()</span>
<span class="n">client</span><span class="p">.</span><span class="n">delete_session</span><span class="p">(</span><span class="s">"my-session-2"</span><span class="p">)</span>

<span class="c1"># Connect mode -- attach to an existing Spark Connect server
</span><span class="n">spark</span> <span class="o">=</span> <span class="n">client</span><span class="p">.</span><span class="n">connect</span><span class="p">(</span><span class="n">base_url</span><span class="o">=</span><span class="s">"sc://spark-server:15002"</span><span class="p">)</span>
<span class="n">df</span> <span class="o">=</span> <span class="n">spark</span><span class="p">.</span><span class="n">sql</span><span class="p">(</span><span class="s">"SELECT * FROM my_table"</span><span class="p">)</span>
<span class="n">df</span><span class="p">.</span><span class="n">show</span><span class="p">()</span>
</code></pre></div></div>

<p>Default specifications: Spark 4.0.1, 1 executor, 512Mi memory and 1 CPU per pod, 300 second session timeout.</p>

<blockquote>
  <p><strong>Note:</strong> v0.4.0 focuses on SparkConnect session management. Batch job support via SparkApplication CR (<code class="language-plaintext highlighter-rouge">submit_job</code>, <code class="language-plaintext highlighter-rouge">get_job</code>, <code class="language-plaintext highlighter-rouge">list_jobs</code>) is planned for a future release.</p>
</blockquote>

<h2 id="a-new-home-for-documentation">A New Home for Documentation</h2>

<p>To support the Kubeflow SDK users and contributors, we’ve introduced a dedicated <a href="https://sdk.kubeflow.org">Kubeflow SDK Website</a>. This site includes:</p>

<ul>
  <li><strong><a href="https://sdk.kubeflow.org/en/latest/getting-started/quickstart.html">Quickstart</a>:</strong> Train your first model with Kubeflow SDK</li>
  <li><strong><a href="https://sdk.kubeflow.org/en/latest/train/api.html">API Reference</a>:</strong> Automatically updated documentation for all SDK modules.</li>
  <li><strong><a href="https://sdk.kubeflow.org/en/latest/examples.html">Examples</a>:</strong> Step-by-step guides from local prototyping to remote training.</li>
</ul>

<h2 id="infrastructure--breaking-changes">Infrastructure &amp; Breaking Changes</h2>

<p>This release includes several architectural updates to ensure the SDK remains secure, scalable, and easy to use. Please note the following requirements when upgrading to v0.4.0.</p>

<h3 id="better-isolation-with-namespaced-trainingruntimes">Better Isolation with Namespaced TrainingRuntimes</h3>

<p>Security and multi-tenancy are core to Kubeflow. In v0.4.0, we’ve introduced support for <a href="https://www.kubeflow.org/docs/components/trainer/operator-guides/runtime/#what-is-trainingruntime">Namespaced TrainingRuntimes</a>. This allows platform teams to provide curated training environments at the namespace level, ensuring that one team’s custom training configuration doesn’t interfere with another’s.</p>

<p><strong>Upgrade Note:</strong> The SDK now prioritizes namespaced runtimes over cluster-wide ones. If you have runtimes with duplicate names in different scopes, verify your <code class="language-plaintext highlighter-rouge">TrainerClient</code> calls are targeting the intended resources.</p>

<h3 id="furthering-parity-between-local-and-remote-execution">Furthering Parity Between Local and Remote Execution</h3>

<p>One of the biggest hurdles in MLOps is the “it worked on my machine” syndrome. With the addition of Dataset and Model Initializers for the <code class="language-plaintext highlighter-rouge">ContainerBackend</code>, the SDK now emulates how Kubernetes handles data dependencies.</p>

<p>Whether you are running locally on Docker or at scale on a cluster, the SDK now automatically manages the “plumbing” of mounting and initializing your data. This ensures your local development environment mirrors the data-loading behavior of your production training jobs.</p>

<h3 id="required-upgrading-to-python-310">Required: Upgrading to Python 3.10+</h3>

<p>To maintain a secure and performant codebase, Kubeflow SDK v0.4.0 is officially moving its minimum requirement to <a href="https://peps.python.org/pep-0619/">Python 3.10</a>.</p>

<p>This change ensures that all SDK users benefit from better security patches, improved type-hinting, and more efficient asynchronous networking for our API clients.</p>

<p><strong>To Upgrade:</strong> Ensure your local environment, Notebook images, and CI/CD pipelines are running Python 3.10 or higher before running <code class="language-plaintext highlighter-rouge">pip install --upgrade kubeflow</code></p>

<h2 id="whats-next-for-kubeflow-sdk">What’s Next for Kubeflow SDK</h2>

<p>Looking ahead, the Kubeflow SDK <a href="https://github.com/kubeflow/sdk/pull/326">2026 Roadmap</a> outlines several exciting initiatives:</p>

<ul>
  <li><strong>Kubeflow MCP Server</strong> to enable AI-assisted interactions with Kubeflow resources</li>
  <li><strong>OpenTelemetry integration</strong> for improved observability across SDK operations</li>
  <li><strong>MLflow support</strong> for experiment tracking and metrics</li>
  <li><strong>First class support for Kubeflow Pipelines</strong> to bring KFP into the unified SDK</li>
  <li><strong>TrainJob checkpointing and dynamic LLM Trainers</strong> for more flexible and resilient training workflows</li>
  <li><strong>End-to-end AI pipelines</strong> orchestrating data processing, training, and optimization using SparkClient, TrainerClient, and OptimizerClient</li>
  <li><strong>Multi-cluster job submission</strong> leveraging Kueue and Multi-Kueue capabilities for Spark and training workloads</li>
  <li><strong>Batch Spark job support</strong> via SparkApplication CR for submit, get, and list operations</li>
</ul>

<p>We encourage the community to review and contribute to the roadmap.</p>

<h2 id="get-involved">Get Involved!</h2>

<p>The Kubeflow SDK is built by and for the community. We welcome contributions, feedback, and participation from everyone! We want to thank the community for their contributions to this release. We invite you to:</p>

<ul>
  <li><strong>Try it out:</strong> <code class="language-plaintext highlighter-rouge">pip install kubeflow==0.4.0</code></li>
  <li><strong>Contribute:</strong>
    <ul>
      <li>Read the <a href="https://github.com/kubeflow/sdk/blob/main/CONTRIBUTING.md">Contributing Guide</a>.</li>
      <li>Browse the <a href="https://github.com/kubeflow/sdk/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22">good first issues</a></li>
      <li>Explore the <a href="https://github.com/kubeflow/sdk">GitHub Repository</a></li>
    </ul>
  </li>
</ul>

<p><strong>Connect with the Community:</strong></p>
<ul>
  <li>Join <a href="https://cloud-native.slack.com/archives/C08KJBVDH5H">#kubeflow-sdk</a> on <a href="https://www.kubeflow.org/docs/about/community/#kubeflow-slack-channels">CNCF Slack</a></li>
  <li>Attend the <a href="https://www.kubeflow.org/docs/about/community/#kubeflow-community-calendars">Kubeflow SDK and ML Experience WG meetings</a></li>
</ul>

<p><strong>Learn More</strong></p>
<ul>
  <li>Visit the <a href="https://sdk.kubeflow.org">Kubeflow SDK Website</a></li>
  <li>View the full <a href="https://github.com/kubeflow/sdk/releases/tag/0.4.0">Changelog</a>.</li>
</ul>

<p><strong>Headed to <a href="https://events.linuxfoundation.org/kubecon-cloudnativecon-europe/">KubeCon + CloudNativeCon 2026 EU</a>?</strong> Stop by the Kubeflow booth to see these features in action!</p>
