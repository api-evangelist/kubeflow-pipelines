---
title: "Introducing the Kubeflow SDK: A Pythonic API to Run AI Workloads at Scale"
url: "https://blog.kubeflow.org/sdk/intro/"
date: "2025-11-07T00:00:00-06:00"
author: "Kubeflow SDK Team"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<blockquote>
  <p><strong>⚡ We want your feedback!</strong> Help shape the future of Kubeflow SDK by taking our <a href="https://docs.google.com/forms/d/e/1FAIpQLSet_IAFQzMMDWolzFt5LI9lhzqOOStjIGHxgYqKBnVcRtDfrw/viewform?usp=dialog">quick survey</a>.</p>
</blockquote>

<h1 id="unified-sdk-concept">Unified SDK Concept</h1>

<p>Scaling AI workloads shouldn’t require deep expertise in distributed systems and container orchestration. Whether you are prototyping on local hardware or deploying to a production Kubernetes cluster, you need a unified API that abstracts infrastructure complexity while preserving flexibility. That’s exactly what the Kubeflow Python SDK delivers.</p>

<p>As an AI Practitioner, you’ve probably experienced this frustrating journey: you start by prototyping locally, training your model on your laptop. When you need more compute power, you have to rewrite everything for distributed training. You containerize your code, rebuild images for every small change, write Kubernetes YAMLs, wrestle with kubectl, and juggle multiple SDKs — one for training, another for hyperparameter tuning, and yet another for pipelines. Each step demands different tools, APIs, and mental models.</p>

<p>All this complexity slows down productivity, drains focus, and ultimately holds back AI innovation. What if there was a better way?</p>

<p>The Kubeflow community started the <strong>Kubeflow SDK &amp; ML Experience Working Group</strong> (WG) in order to address these challenges. You can find more information about this WG on our <a href="https://youtu.be/VkbVVk2OGUI?list=PLmzRWLV1CK_wSO2IMPnzChxESmaoXNfrY">YouTube playlist</a>.</p>

<h1 id="introducing-kubeflow-sdk">Introducing Kubeflow SDK</h1>

<p>The SDK sits on top of the Kubeflow ecosystem as a unified interface layer. When you write Python code, the SDK translates it into the appropriate Kubernetes resources — generating CRs, handling orchestration, and managing distributed communication. You get all the power of Kubeflow and distributed AI compute without needing to understand Kubernetes.</p>

<p><img alt="kubeflow ecosystem" src="https://blog.kubeflow.org/images/2025-11-07-introducing-kubeflow-sdk/kubeflow-sdk.drawio.svg" /></p>

<p>Getting started is simple:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="n">pip</span> <span class="n">install</span> <span class="n">kubeflow</span>
</code></pre></div></div>
<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">TrainerClient</span>

<span class="k">def</span> <span class="nf">train_model</span><span class="p">():</span>
    <span class="kn">import</span> <span class="nn">torch</span>

    <span class="n">model</span> <span class="o">=</span> <span class="n">torch</span><span class="p">.</span><span class="n">nn</span><span class="p">.</span><span class="n">Linear</span><span class="p">(</span><span class="mi">10</span><span class="p">,</span> <span class="mi">1</span><span class="p">)</span>
    <span class="n">optimizer</span> <span class="o">=</span> <span class="n">torch</span><span class="p">.</span><span class="n">optim</span><span class="p">.</span><span class="n">Adam</span><span class="p">(</span><span class="n">model</span><span class="p">.</span><span class="n">parameters</span><span class="p">())</span>

    <span class="c1"># Training loop
</span>    <span class="k">for</span> <span class="n">epoch</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">10</span><span class="p">):</span>
        <span class="c1"># Your training logic
</span>        <span class="k">pass</span>

    <span class="n">torch</span><span class="p">.</span><span class="n">save</span><span class="p">(</span><span class="n">model</span><span class="p">.</span><span class="n">state_dict</span><span class="p">(),</span> <span class="s">"model.pt"</span><span class="p">)</span>

<span class="c1"># Create a client and train
</span><span class="n">client</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">()</span>
<span class="n">client</span><span class="p">.</span><span class="n">train</span><span class="p">(</span><span class="n">train_func</span><span class="o">=</span><span class="n">train_model</span><span class="p">)</span>
</code></pre></div></div>

<p>The following principles are the foundation that guide the design and implementation of the SDK:</p>

<ul>
  <li><strong>Unified Experience</strong>: Single SDK to interact with multiple Kubeflow projects through consistent Python APIs</li>
  <li><strong>Simplified AI Workloads</strong>: Abstract away Kubernetes complexity and work effortlessly across all Kubeflow projects using familiar Python APIs</li>
  <li><strong>Built for Scale</strong>: Seamlessly scale any AI workload — from local laptop to large-scale production cluster with thousands of GPUs using the same APIs.</li>
  <li><strong>Rapid Iteration</strong>: Reduced friction between development and production environments</li>
  <li><strong>Local Development</strong>: First-class support for local development without a Kubernetes cluster requiring only pip installation</li>
</ul>

<h2 id="role-in-the-kubeflow-ecosystem">Role in the Kubeflow Ecosystem</h2>

<p>The SDK doesn’t replace any Kubeflow projects — it provides a unified way to use them. Kubeflow Trainer, Katib, Spark Operator, Pipelines, etc still handle the actual workload execution. The SDK makes them easier to interact with through consistent Python APIs, letting you work entirely in the language you already use for ML development.</p>

<p>This creates a clear separation:</p>
<ul>
  <li><strong>AI Practitioners</strong> use the SDK to submit jobs and manage workflows through Python, without touching YAML or Kubernetes directly</li>
  <li><strong>Platform Administrators</strong> continue managing infrastructure — installing components, configuring runtimes, setting resource quotas. Nothing changes on the infrastructure side.</li>
</ul>

<p><img alt="kubeflow user personas" src="https://blog.kubeflow.org/images/2025-11-07-introducing-kubeflow-sdk/user-personas.drawio.svg" /></p>

<p>The Kubeflow SDK works with your existing Kubeflow deployment. If you already have Kubeflow Trainer and Katib installed, just <code class="language-plaintext highlighter-rouge">pip install kubeflow</code> and start using them through the unified interface. As Kubeflow evolves with new components and features, the SDK provides a stable Python layer that adapts alongside the ecosystem.</p>

<table>
  <thead>
    <tr>
      <th>Project</th>
      <th>Status</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Kubeflow Trainer</td>
      <td>Available ✅</td>
      <td>Train and fine-tune AI models with various frameworks</td>
    </tr>
    <tr>
      <td>Kubeflow Optimizer</td>
      <td>Available ✅</td>
      <td>Hyperparameter optimization</td>
    </tr>
    <tr>
      <td>Kubeflow Pipelines</td>
      <td>Planned 🚧</td>
      <td>Build, run, and track AI workflows</td>
    </tr>
    <tr>
      <td>Kubeflow Model Registry</td>
      <td>Planned 🚧</td>
      <td>Manage model artifacts, versions and ML artifacts metadata</td>
    </tr>
    <tr>
      <td>Kubeflow Spark Operator</td>
      <td>Planned 🚧</td>
      <td>Manage Spark applications for data processing and feature engineering</td>
    </tr>
  </tbody>
</table>

<h1 id="key-features">Key Features</h1>

<h2 id="unified-python-interface">Unified Python Interface</h2>

<p>The SDK provides a consistent experience across all Kubeflow components. Whether you’re training models or optimizing hyperparameters, the APIs follow the same patterns:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">TrainerClient</span>
<span class="kn">from</span> <span class="nn">kubeflow.optimizer</span> <span class="kn">import</span> <span class="n">OptimizerClient</span>

<span class="c1"># Initialize clients
</span><span class="n">trainer</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">()</span>
<span class="n">optimizer</span> <span class="o">=</span> <span class="n">OptimizerClient</span><span class="p">()</span>

<span class="c1"># List jobs
</span><span class="n">TrainerClient</span><span class="p">().</span><span class="n">list_jobs</span><span class="p">()</span>
<span class="n">OptimizerClient</span><span class="p">().</span><span class="n">list_jobs</span><span class="p">()</span>
</code></pre></div></div>

<h2 id="trainer-client">Trainer Client</h2>

<p>The TrainerClient provides the easiest way to run distributed training on Kubernetes, built on top of <a href="https://blog.kubeflow.org/trainer/intro/">Kubeflow Trainer v2</a>. Whether you’re training custom models with PyTorch, or fine-tuning LLMs, the client provides a Python API for submitting and monitoring training jobs at scale.</p>

<p>The client works with pre-configured runtimes that Platform Administrators set up. These runtimes define the container images, resource policies, and infrastructure settings. As an AI Practitioner, you reference these runtimes and focus on your training code:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">TrainerClient</span><span class="p">,</span> <span class="n">CustomTrainer</span>

<span class="k">def</span> <span class="nf">get_torch_dist</span><span class="p">():</span>
    <span class="s">"""Your PyTorch training code runs on each node."""</span>
    <span class="kn">import</span> <span class="nn">os</span>
    <span class="kn">import</span> <span class="nn">torch</span>
    <span class="kn">import</span> <span class="nn">torch.distributed</span> <span class="k">as</span> <span class="n">dist</span>

    <span class="n">dist</span><span class="p">.</span><span class="n">init_process_group</span><span class="p">(</span><span class="n">backend</span><span class="o">=</span><span class="s">"gloo"</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="s">"PyTorch Distributed Environment"</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"WORLD_SIZE: </span><span class="si">{</span><span class="n">dist</span><span class="p">.</span><span class="n">get_world_size</span><span class="p">()</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"RANK: </span><span class="si">{</span><span class="n">dist</span><span class="p">.</span><span class="n">get_rank</span><span class="p">()</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"LOCAL_RANK: </span><span class="si">{</span><span class="n">os</span><span class="p">.</span><span class="n">environ</span><span class="p">[</span><span class="s">'LOCAL_RANK'</span><span class="p">]</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>

<span class="c1"># Create the TrainJob
</span><span class="n">job_id</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">().</span><span class="n">train</span><span class="p">(</span>
    <span class="n">runtime</span><span class="o">=</span><span class="n">TrainerClient</span><span class="p">().</span><span class="n">get_runtime</span><span class="p">(</span><span class="s">"torch-distributed"</span><span class="p">),</span>
    <span class="n">trainer</span><span class="o">=</span><span class="n">CustomTrainer</span><span class="p">(</span>
        <span class="n">func</span><span class="o">=</span><span class="n">get_torch_dist</span><span class="p">,</span>
        <span class="n">num_nodes</span><span class="o">=</span><span class="mi">3</span><span class="p">,</span>
        <span class="n">resources_per_node</span><span class="o">=</span><span class="p">{</span>
            <span class="s">"cpu"</span><span class="p">:</span> <span class="mi">2</span><span class="p">,</span>
        <span class="p">},</span>
    <span class="p">),</span>
<span class="p">)</span>

<span class="c1"># Wait for TrainJob to complete
</span><span class="n">TrainerClient</span><span class="p">().</span><span class="n">wait_for_job_status</span><span class="p">(</span><span class="n">job_id</span><span class="p">)</span>

<span class="c1"># Print TrainJob logs
</span><span class="k">print</span><span class="p">(</span><span class="s">"</span><span class="se">\n</span><span class="s">"</span><span class="p">.</span><span class="n">join</span><span class="p">(</span><span class="n">TrainerClient</span><span class="p">().</span><span class="n">get_job_logs</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="n">job_id</span><span class="p">)))</span>
</code></pre></div></div>

<p>The TrainerClient supports <code class="language-plaintext highlighter-rouge">CustomTrainer</code> for your own training logic and <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/builtin-trainer/torchtune/"><code class="language-plaintext highlighter-rouge">BuiltinTrainer</code></a> for pre-packaged training patterns like LLM fine-tuning.</p>

<p>Getting started with LLM fine-tuning is as simple as a single line. The default model, dataset, and training configurations are pre-baked into the runtime:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="n">TrainerClient</span><span class="p">().</span><span class="n">train</span><span class="p">(</span>
    <span class="n">runtime</span><span class="o">=</span><span class="n">TrainerClient</span><span class="p">().</span><span class="n">get_runtime</span><span class="p">(</span><span class="s">"torchtune-qwen2.5-1.5b"</span><span class="p">),</span>
<span class="p">)</span>
</code></pre></div></div>

<p>You can also customize every aspect of the fine-tuning process — specify your own dataset, model, LoRA configuration, and training hyperparameters:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">TrainerClient</span><span class="p">,</span> <span class="n">BuiltinTrainer</span><span class="p">,</span> <span class="n">TorchTuneConfig</span>
<span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">Initializer</span><span class="p">,</span> <span class="n">HuggingFaceDatasetInitializer</span><span class="p">,</span> <span class="n">HuggingFaceModelInitializer</span>
<span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">TorchTuneInstructDataset</span><span class="p">,</span> <span class="n">LoraConfig</span><span class="p">,</span> <span class="n">DataFormat</span>

<span class="n">client</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">()</span>

<span class="n">client</span><span class="p">.</span><span class="n">train</span><span class="p">(</span>
    <span class="n">runtime</span><span class="o">=</span><span class="n">client</span><span class="p">.</span><span class="n">get_runtime</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="s">"torchtune-llama3.2-1b"</span><span class="p">),</span>
    <span class="n">initializer</span><span class="o">=</span><span class="n">Initializer</span><span class="p">(</span>
        <span class="n">dataset</span><span class="o">=</span><span class="n">HuggingFaceDatasetInitializer</span><span class="p">(</span>
            <span class="n">storage_uri</span><span class="o">=</span><span class="s">"hf://tatsu-lab/alpaca/data"</span>
        <span class="p">),</span>
        <span class="n">model</span><span class="o">=</span><span class="n">HuggingFaceModelInitializer</span><span class="p">(</span>
            <span class="n">storage_uri</span><span class="o">=</span><span class="s">"hf://meta-llama/Llama-3.2-1B-Instruct"</span><span class="p">,</span>
            <span class="n">access_token</span><span class="o">=</span><span class="s">"hf_..."</span><span class="p">,</span>
        <span class="p">)</span>
    <span class="p">),</span>
    <span class="n">trainer</span><span class="o">=</span><span class="n">BuiltinTrainer</span><span class="p">(</span>
        <span class="n">config</span><span class="o">=</span><span class="n">TorchTuneConfig</span><span class="p">(</span>
            <span class="n">dataset_preprocess_config</span><span class="o">=</span><span class="n">TorchTuneInstructDataset</span><span class="p">(</span>
                <span class="n">source</span><span class="o">=</span><span class="n">DataFormat</span><span class="p">.</span><span class="n">PARQUET</span><span class="p">,</span>
            <span class="p">),</span>
            <span class="n">peft_config</span><span class="o">=</span><span class="n">LoraConfig</span><span class="p">(</span>
                <span class="n">apply_lora_to_mlp</span><span class="o">=</span><span class="bp">True</span><span class="p">,</span>
                <span class="n">lora_attn_modules</span><span class="o">=</span><span class="p">[</span><span class="s">"q_proj"</span><span class="p">,</span> <span class="s">"k_proj"</span><span class="p">,</span> <span class="s">"v_proj"</span><span class="p">,</span> <span class="s">"output_proj"</span><span class="p">],</span>
                <span class="n">quantize_base</span><span class="o">=</span><span class="bp">True</span><span class="p">,</span>
            <span class="p">),</span>
            <span class="n">resources_per_node</span><span class="o">=</span><span class="p">{</span>
                <span class="s">"gpu"</span><span class="p">:</span> <span class="mi">1</span><span class="p">,</span>
            <span class="p">}</span>
        <span class="p">)</span>
    <span class="p">)</span>
<span class="p">)</span>
</code></pre></div></div>

<p>You can mix and match — use the runtime’s default model but specify your own dataset, or keep the default dataset but customize the LoRA parameters. The Initializers download datasets and models once to shared storage, then all training pods access the data from there — reducing startup time and network usage.</p>

<p>For more details about Kubeflow Trainer capabilities, including gang-scheduling, fault tolerance, and MPI support, check out the <a href="https://blog.kubeflow.org/trainer/intro/">Kubeflow Trainer v2 blog post</a>.</p>

<h2 id="optimizer-client">Optimizer Client</h2>

<p>The OptimizerClient manages hyperparameter optimization for large models of any size on Kubernetes. With consistent APIs across TrainerClient and OptimizerClient, you can easily transition from training to optimization — define your training job template once, specify which parameters to optimize, and the client orchestrates multiple trials to find the best hyperparameter configuration. This consistent API design significantly enhances the user experience during AI development.</p>

<p>The client launches trials in parallel according to your resource constraints, tracks metrics across experiments, and identifies optimal parameters.</p>

<p>First, define your training job template:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">TrainerClient</span><span class="p">,</span> <span class="n">CustomTrainer</span>
<span class="kn">from</span> <span class="nn">kubeflow.optimizer</span> <span class="kn">import</span> <span class="n">OptimizerClient</span><span class="p">,</span> <span class="n">TrainJobTemplate</span><span class="p">,</span> <span class="n">Search</span><span class="p">,</span> <span class="n">Objective</span><span class="p">,</span> <span class="n">TrialConfig</span>

<span class="k">def</span> <span class="nf">train_func</span><span class="p">(</span><span class="n">learning_rate</span><span class="p">:</span> <span class="nb">float</span><span class="p">,</span> <span class="n">batch_size</span><span class="p">:</span> <span class="nb">int</span><span class="p">):</span>
    <span class="s">"""Training function with hyperparameters."""</span>
    <span class="c1"># Your training code here
</span>    <span class="kn">import</span> <span class="nn">time</span>
    <span class="kn">import</span> <span class="nn">random</span>

    <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="mi">10</span><span class="p">):</span>
        <span class="n">time</span><span class="p">.</span><span class="n">sleep</span><span class="p">(</span><span class="mi">1</span><span class="p">)</span>
        <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"Training </span><span class="si">{</span><span class="n">i</span><span class="si">}</span><span class="s">, lr: </span><span class="si">{</span><span class="n">learning_rate</span><span class="si">}</span><span class="s">, batch_size: </span><span class="si">{</span><span class="n">batch_size</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>

    <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"loss=</span><span class="si">{</span><span class="nb">round</span><span class="p">(</span><span class="n">random</span><span class="p">.</span><span class="n">uniform</span><span class="p">(</span><span class="mf">0.77</span><span class="p">,</span> <span class="mf">0.99</span><span class="p">),</span> <span class="mi">2</span><span class="p">)</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>


<span class="c1"># Create a reusable template
</span><span class="n">template</span> <span class="o">=</span> <span class="n">TrainJobTemplate</span><span class="p">(</span>
    <span class="n">trainer</span><span class="o">=</span><span class="n">CustomTrainer</span><span class="p">(</span>
        <span class="n">func</span><span class="o">=</span><span class="n">train_func</span><span class="p">,</span>
        <span class="n">func_args</span><span class="o">=</span><span class="p">{</span><span class="s">"learning_rate"</span><span class="p">:</span> <span class="s">"0.01"</span><span class="p">,</span> <span class="s">"batch_size"</span><span class="p">:</span> <span class="s">"16"</span><span class="p">},</span>
        <span class="n">num_nodes</span><span class="o">=</span><span class="mi">2</span><span class="p">,</span>
        <span class="n">resources_per_node</span><span class="o">=</span><span class="p">{</span><span class="s">"gpu"</span><span class="p">:</span> <span class="mi">1</span><span class="p">},</span>
    <span class="p">),</span>
    <span class="n">runtime</span><span class="o">=</span><span class="n">TrainerClient</span><span class="p">().</span><span class="n">get_runtime</span><span class="p">(</span><span class="s">"torch-distributed"</span><span class="p">),</span>
<span class="p">)</span>

<span class="c1"># Verify that your TrainJob is working with test hyperparameters.
</span><span class="n">TrainerClient</span><span class="p">().</span><span class="n">train</span><span class="p">(</span><span class="o">**</span><span class="n">template</span><span class="p">)</span>
</code></pre></div></div>

<p>Then optimize hyperparameters with a single call:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="n">optimizer</span> <span class="o">=</span> <span class="n">OptimizerClient</span><span class="p">()</span>

<span class="n">job_name</span> <span class="o">=</span> <span class="n">optimizer</span><span class="p">.</span><span class="n">optimize</span><span class="p">(</span>
    <span class="c1"># The same template can be used for Hyperparameter Optimisation
</span>    <span class="n">trial_template</span><span class="o">=</span><span class="n">template</span><span class="p">,</span>
    <span class="n">search_space</span><span class="o">=</span><span class="p">{</span>
        <span class="s">"learning_rate"</span><span class="p">:</span> <span class="n">Search</span><span class="p">.</span><span class="n">loguniform</span><span class="p">(</span><span class="mf">0.001</span><span class="p">,</span> <span class="mf">0.1</span><span class="p">),</span>
        <span class="s">"batch_size"</span><span class="p">:</span> <span class="n">Search</span><span class="p">.</span><span class="n">choice</span><span class="p">([</span><span class="mi">16</span><span class="p">,</span> <span class="mi">32</span><span class="p">,</span> <span class="mi">64</span><span class="p">,</span> <span class="mi">128</span><span class="p">]),</span>
    <span class="p">},</span>
    <span class="n">trial_config</span><span class="o">=</span><span class="n">TrialConfig</span><span class="p">(</span>
        <span class="n">num_trials</span><span class="o">=</span><span class="mi">20</span><span class="p">,</span>
        <span class="n">parallel_trials</span><span class="o">=</span><span class="mi">4</span><span class="p">,</span>
        <span class="n">max_failed_trials</span><span class="o">=</span><span class="mi">5</span><span class="p">,</span>
    <span class="p">),</span>
<span class="p">)</span>

<span class="c1"># Verify OptimizationJob was created
</span><span class="n">optimizer</span><span class="p">.</span><span class="n">get_job</span><span class="p">(</span><span class="n">job_name</span><span class="p">)</span>

<span class="c1"># Wait for OptimizationJob to complete
</span><span class="n">optimizer</span><span class="p">.</span><span class="n">wait_for_job_status</span><span class="p">(</span><span class="n">job_name</span><span class="p">)</span>

<span class="c1"># Get the best hyperparameters and metrics from an OptimizationJob
</span><span class="n">best_results</span> <span class="o">=</span> <span class="n">optimizer</span><span class="p">.</span><span class="n">get_best_results</span><span class="p">(</span><span class="n">job_name</span><span class="p">)</span>
<span class="k">print</span><span class="p">(</span><span class="n">best_results</span><span class="p">)</span>
<span class="c1"># Output:
# Result(
#     parameters={'learning_rate': '0.0234', 'batch_size': '64'},
#     metrics=[Metric(name='loss', min='0.78', max='0.78', latest='0.78')]
# )
</span>
<span class="c1"># See all the trials (TrainJobs) created during optimization
</span><span class="n">job</span> <span class="o">=</span> <span class="n">optimizer</span><span class="p">.</span><span class="n">get_job</span><span class="p">(</span><span class="n">job_name</span><span class="p">)</span>
<span class="k">print</span><span class="p">(</span><span class="n">job</span><span class="p">.</span><span class="n">trials</span><span class="p">)</span>
</code></pre></div></div>

<p>This creates multiple TrainJob instances (trials) with different hyperparameter combinations, executes them in parallel based on available resources, and tracks which parameters produce the best results. Each trial is a full training job managed by Kubeflow Trainer. Using <a href="https://www.kubeflow.org/docs/components/katib/user-guides/katib-ui/">Katib UI</a>, you can visualize your optimization with an interactive graph that shows metric performance against hyperparameter values across all trials.</p>

<p><img alt="Katib UI example" src="https://blog.kubeflow.org/images/2025-11-07-introducing-kubeflow-sdk/katib-ui.png" /></p>

<p>For more details about hyperparameter optimization, check out the <a href="https://github.com/kubeflow/sdk/tree/main/docs/proposals/46-hyperparameter-optimization">OptimizerClient KEP</a>.</p>

<h2 id="local-execution-mode">Local Execution Mode</h2>

<p>Local Execution Mode provides backend flexibility while maintaining full API compatibility with the Kubernetes backend, substantially reducing friction for AI practitioners when developing and iterating.</p>

<p>Choose the right execution environment for your stage of development:</p>

<h3 id="local-process-backend-fastest-iteration">Local Process Backend: Fastest Iteration</h3>

<p>The Local Process Backend is your starting point for ML development - offering the fastest possible iteration cycle with zero infrastructure overhead. This backend executes your training code directly as a Python subprocess on your local machine, bypassing containers, orchestration, and network complexity entirely.</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer.backends.localprocess</span> <span class="kn">import</span> <span class="n">LocalProcessBackendConfig</span>

<span class="n">config</span> <span class="o">=</span> <span class="n">LocalProcessBackendConfig</span><span class="p">()</span>
<span class="n">client</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">(</span><span class="n">config</span><span class="p">)</span>

<span class="c1"># Runs directly on your machine - no containers, no cluster
</span><span class="n">client</span><span class="p">.</span><span class="n">train</span><span class="p">(</span><span class="n">train_func</span><span class="o">=</span><span class="n">train_model</span><span class="p">)</span>
</code></pre></div></div>

<h3 id="container-backend-production-like-environment">Container Backend: Production-Like Environment</h3>

<p>The Container Backend bridges the gap between local development and production deployment by bringing production parity to your laptop. This backend executes your training code inside containers (using Docker or Podman), ensuring that your development environment matches your production environment byte-for-byte - same dependencies, same Python version, same system libraries, same everything.</p>

<p>Docker Example:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer.backends.container</span> <span class="kn">import</span> <span class="n">ContainerBackendConfig</span>

<span class="n">config</span> <span class="o">=</span> <span class="n">ContainerBackendConfig</span><span class="p">(</span>
    <span class="n">container_runtime</span><span class="o">=</span><span class="s">"docker"</span><span class="p">,</span>
    <span class="n">auto_remove</span><span class="o">=</span><span class="bp">True</span>  <span class="c1"># Clean up containers after completion
</span><span class="p">)</span>

<span class="n">client</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">(</span><span class="n">config</span><span class="p">)</span>

<span class="c1"># Launch 2-node distributed training locally
</span><span class="n">client</span><span class="p">.</span><span class="n">train</span><span class="p">(</span><span class="n">train_func</span><span class="o">=</span><span class="n">train_model</span><span class="p">,</span> <span class="n">num_nodes</span><span class="o">=</span><span class="mi">2</span><span class="p">)</span>
</code></pre></div></div>

<p>Podman Example:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer.backends.container</span> <span class="kn">import</span> <span class="n">ContainerBackendConfig</span>

<span class="n">config</span> <span class="o">=</span> <span class="n">ContainerBackendConfig</span><span class="p">(</span>
    <span class="n">container_runtime</span><span class="o">=</span><span class="s">"podman"</span><span class="p">,</span>
    <span class="n">auto_remove</span><span class="o">=</span><span class="bp">True</span>
<span class="p">)</span>

<span class="n">client</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">(</span><span class="n">config</span><span class="p">)</span>
<span class="n">client</span><span class="p">.</span><span class="n">train</span><span class="p">(</span><span class="n">train_func</span><span class="o">=</span><span class="n">train_model</span><span class="p">,</span> <span class="n">num_nodes</span><span class="o">=</span><span class="mi">2</span><span class="p">)</span>
</code></pre></div></div>

<h3 id="kubernetes-backend-production-scale">Kubernetes Backend: Production Scale</h3>

<p>The Kubernetes Backend enables Kubeflow SDK to perform reliably at production scale - enabling you to deploy the exact same training code you developed locally to a production Kubernetes cluster with massive computational resources. This backend transforms your simple <code class="language-plaintext highlighter-rouge">client.train()</code> call into a full-fledged distributed training job managed by Kubeflow’s Trainer, complete with fault tolerance, resource scheduling, and cluster-wide orchestration.</p>

<p>Kubernetes Example:</p>

<div class="language-python highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">kubeflow.trainer.backends.kubernetes</span> <span class="kn">import</span> <span class="n">KubernetesBackendConfig</span>

<span class="n">config</span> <span class="o">=</span> <span class="n">KubernetesBackendConfig</span><span class="p">(</span>
    <span class="n">namespace</span><span class="o">=</span><span class="s">"ml-training"</span><span class="p">,</span>
<span class="p">)</span>

<span class="n">client</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">(</span><span class="n">config</span><span class="p">)</span>

<span class="c1"># Scales to hundreds of nodes - the same code you tested locally
</span><span class="n">client</span><span class="p">.</span><span class="n">train</span><span class="p">(</span>
    <span class="n">train_func</span><span class="o">=</span><span class="n">train_model</span><span class="p">,</span>
    <span class="n">num_nodes</span><span class="o">=</span><span class="mi">100</span><span class="p">,</span>
    <span class="n">packages_to_install</span><span class="o">=</span><span class="p">[</span><span class="s">"torch"</span><span class="p">,</span> <span class="s">"transformers"</span><span class="p">]</span>
<span class="p">)</span>
</code></pre></div></div>

<h1 id="whats-next">What’s Next?</h1>

<p>We’re just getting started. The Kubeflow SDK currently supports Trainer and Optimizer, but the vision is much bigger — a unified Python interface for the entire <a href="https://www.kubeflow.org/docs/started/architecture/#kubeflow-projects-in-the-ai-lifecycle">Cloud Native AI Lifecycle</a>.</p>

<p>Here’s what’s on the horizon:</p>

<ul>
  <li><a href="https://github.com/kubeflow/sdk/issues/125"><strong>Pipelines Integration</strong></a>: A PipelinesClient to build end-to-end ML workflows. Pipelines will reuse the core Kubeflow SDK primitives for training, optimization, and deployment in a single pipeline. The Kubeflow SDK will also power <a href="https://github.com/kubeflow/pipelines-components">KFP core components</a></li>
  <li><a href="https://github.com/kubeflow/sdk/issues/59"><strong>Model Registry Integration</strong></a>: Seamlessly manage model artifacts and versions across the training and serving lifecycle</li>
  <li><a href="https://github.com/kubeflow/sdk/issues/107"><strong>Spark Operator Integration</strong></a>: Data processing and feature engineering through a SparkClient interface</li>
  <li><a href="https://github.com/kubeflow/sdk/issues/50"><strong>Documentation</strong></a>: Full Kubeflow SDK documentation with guides, examples, and API references</li>
  <li><a href="https://github.com/kubeflow/sdk/issues/153"><strong>Local Execution for Optimizer</strong></a>: Run hyperparameter optimization experiments locally before scaling to Kubernetes</li>
  <li><a href="https://github.com/kubeflow/sdk/issues/48"><strong>Workspace Snapshots</strong></a>: Capture your entire development environment and reproduce it in distributed training jobs</li>
  <li><a href="https://github.com/kubeflow/sdk/issues/23"><strong>Multi-Cluster Support</strong></a>: Manage training jobs across multiple Kubernetes clusters from a single SDK interface</li>
  <li><a href="https://github.com/kubeflow/trainer/issues/2655"><strong>Distributed Data Cache</strong></a>: In-memory caching for large datasets via initializer SDK configuration</li>
  <li><a href="https://github.com/kubeflow/trainer/issues/2752"><strong>Additional Built-in Trainers</strong></a>: Support for more fine-tuning frameworks beyond TorchTune — <a href="https://github.com/unslothai/unsloth">Unsloth</a>, <a href="https://github.com/meta-pytorch/torchforge">torchforge</a>, <a href="https://github.com/axolotl-ai-cloud/axolotl">Axolotl</a>, <a href="https://github.com/hiyouga/LLaMA-Factory">LLaMA-Factory</a>, and others</li>
</ul>

<p>The community is driving these features forward. If you have ideas, feedback, or want to contribute, we’d love to hear from you!</p>

<h1 id="get-involved">Get Involved</h1>

<p>The Kubeflow SDK is built by and for the community. We welcome contributions, feedback, and participation from everyone!</p>

<p><strong>🔔 Help Shape the Future of Kubeflow SDK</strong></p>

<p>We want to hear from you! Take our <a href="https://docs.google.com/forms/d/e/1FAIpQLSet_IAFQzMMDWolzFt5LI9lhzqOOStjIGHxgYqKBnVcRtDfrw/viewform?usp=dialog">Kubeflow Unified SDK Survey</a> 
to help us understand your biggest pain points and identify which new features will provide the most value to you and 
your team. Your feedback directly influences our roadmap and priorities.</p>

<p><strong>Resources</strong>:</p>
<ul>
  <li><a href="https://github.com/kubeflow/sdk">GitHub Repo</a></li>
  <li><a href="https://docs.google.com/document/d/1rX7ELAHRb_lvh0Y7BK1HBYAbA0zi9enB0F_358ZC58w/edit?tab=t.0#heading=h.e0573r7wwkgl">Kubeflow SDK design document</a></li>
</ul>

<p><strong>Connect with the Community</strong>:</p>
<ul>
  <li>Join <a href="https://cloud-native.slack.com/archives/C08KJBVDH5H">#kubeflow-ml-experience</a> on <a href="https://www.kubeflow.org/docs/about/community/#kubeflow-slack-channels">CNCF Slack</a></li>
  <li>Attend the <a href="https://bit.ly/kf-ml-experience">Kubeflow SDK and ML Experience WG</a> meetings</li>
  <li>Check out <a href="https://github.com/kubeflow/sdk/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22">good first issues</a> to get started</li>
</ul>
