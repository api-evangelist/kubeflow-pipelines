---
title: "Kubeflow Trainer v2.2: JAX &amp; XGBoost Runtimes, Flux for HPC Support, and TrainJob progress and metrics observability"
url: "https://blog.kubeflow.org/kubeflow-trainer-v2.2-release/"
date: "2026-03-20T00:00:00-05:00"
author: "Kubeflow Trainer Team"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<p>Just a little over one week ahead of KubeCon + CloudNativeCon EU 2026, the Kubeflow team is excited to ship Trainer v2.2. The v2.2 release reinforces our commitment to expanding the Kubeflow Trainer ecosystem – meeting developers where they are by adding native support for JAX, XGBoost, and Flux, while also delivering deeper observability into training jobs.</p>

<p>Key highlights of the v2.2 release include:</p>

<ul>
  <li><strong>First-class support for Training Runtimes</strong> for <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/jax/">JAX</a> and <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/xgboost/">XGBoost</a>, enabling native distributed training on Kubernetes. This marks a major milestone for the Trainer project, achieving full compatibility with Training Operator v1 CRDs: PyTorchJob, MPIJob, JAXJob, and XGBoostJob – now unified under a single TrainJob abstraction.</li>
  <li><a href="https://github.com/kubeflow/trainer/tree/master/docs/proposals/2779-trainjob-progress"><strong>Enhanced training observability</strong></a>, allowing progress and metrics to be propagated directly from training scripts to the TrainJob status. <a href="https://github.com/huggingface/transformers/pull/44487">Hugging Face Transformers</a> already integrate with the <em>KubeflowTrainerCallback</em> to automate this capability.</li>
  <li><a href="https://www.kubeflow.org/docs/components/trainer/user-guides/flux/"><strong>Flux runtime support</strong></a>, bringing HPC workloads to Kubernetes and improving MPI bootstrapping within TrainJob.</li>
  <li><a href="https://github.com/kubeflow/trainer/tree/master/docs/proposals/2899-resource-timeouts"><strong>TrainJob activeDeadlineSeconds API</strong></a>, enabling explicit timeout policies for training jobs.</li>
  <li><a href="https://www.kubeflow.org/docs/components/trainer/operator-guides/runtime-patches/"><strong>RuntimePatches API</strong></a>, introducing a more flexible and scalable way to customize runtime configurations from the TrainJobs.</li>
</ul>

<p>You can now install the Kubeflow Trainer control plane and its training runtimes with a single command:</p>

<div class="language-shell highlighter-rouge"><div class="highlight"><pre class="highlight"><code>helm <span class="nb">install </span>kubeflow-trainer oci://ghcr.io/kubeflow/charts/kubeflow-trainer <span class="se">\</span>
    <span class="nt">--namespace</span> kubeflow-system <span class="se">\</span>
    <span class="nt">--create-namespace</span> <span class="se">\</span>
    <span class="nt">--version</span> 2.2.0 <span class="se">\</span>
    <span class="nt">--set</span> runtimes.defaultEnabled<span class="o">=</span><span class="nb">true</span>
</code></pre></div></div>

<h2 id="bringing-jax-to-kubernetes-with-trainer">Bringing JAX to Kubernetes with Trainer</h2>

<p>Kubeflow Trainer supports running JAX workloads on Kubernetes through the <code class="language-plaintext highlighter-rouge">jax-distributed</code> runtime. It is designed for distributed and parallel JAX computation using jax.distributed and SPMD primitives like pmap, pjit, and shard_map. The runtime maps one Kubernetes Pod to one JAX process and injects the required distributed environment variables so training or fine-tuning can run consistently across multiple nodes and devices.</p>

<ul>
  <li>Multi-process CPU training</li>
  <li>Multi-GPU training using CUDA enabled JAX</li>
  <li>Data-parallel and model-parallel JAX workloads</li>
  <li>Massive scale <a href="https://github.com/kubeflow/website/pull/4343">TPU distributed training</a> with ComputeClases</li>
</ul>

<p>Start by following the Getting Started guide for Kubeflow Trainer basics and making sure you have Kubeflow SDK installed on your machine:</p>

<div class="language-shell highlighter-rouge"><div class="highlight"><pre class="highlight"><code>pip <span class="nb">install </span>kubeflow 
</code></pre></div></div>

<p>Use the jax-distributed runtime and initialize JAX distributed explicitly in your training script before any JAX computation:</p>

<div class="language-py highlighter-rouge"><div class="highlight"><pre class="highlight"><code>
<span class="kn">from</span> <span class="nn">kubeflow.trainer</span> <span class="kn">import</span> <span class="n">TrainerClient</span><span class="p">,</span> <span class="n">CustomTrainer</span>

<span class="k">def</span> <span class="nf">get_jax_dist</span><span class="p">():</span>
    <span class="kn">import</span> <span class="nn">os</span>
    <span class="kn">import</span> <span class="nn">jax</span>
    <span class="kn">import</span> <span class="nn">jax.distributed</span> <span class="k">as</span> <span class="n">dist</span>

    <span class="n">dist</span><span class="p">.</span><span class="n">initialize</span><span class="p">(</span>
        <span class="n">coordinator_address</span><span class="o">=</span><span class="n">os</span><span class="p">.</span><span class="n">environ</span><span class="p">[</span><span class="s">"JAX_COORDINATOR_ADDRESS"</span><span class="p">],</span>
        <span class="n">num_processes</span><span class="o">=</span><span class="nb">int</span><span class="p">(</span><span class="n">os</span><span class="p">.</span><span class="n">environ</span><span class="p">[</span><span class="s">"JAX_NUM_PROCESSES"</span><span class="p">]),</span>
        <span class="n">process_id</span><span class="o">=</span><span class="nb">int</span><span class="p">(</span><span class="n">os</span><span class="p">.</span><span class="n">environ</span><span class="p">[</span><span class="s">"JAX_PROCESS_ID"</span><span class="p">]),</span>
    <span class="p">)</span>

    <span class="k">print</span><span class="p">(</span><span class="s">"JAX Distributed Environment"</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"Local devices: </span><span class="si">{</span><span class="n">jax</span><span class="p">.</span><span class="n">local_devices</span><span class="p">()</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="sa">f</span><span class="s">"Global device count: </span><span class="si">{</span><span class="n">jax</span><span class="p">.</span><span class="n">device_count</span><span class="p">()</span><span class="si">}</span><span class="s">"</span><span class="p">)</span>

    <span class="kn">import</span> <span class="nn">jax.numpy</span> <span class="k">as</span> <span class="n">jnp</span>
    <span class="n">x</span> <span class="o">=</span> <span class="n">jnp</span><span class="p">.</span><span class="n">ones</span><span class="p">((</span><span class="mi">4</span><span class="p">,))</span>
    <span class="n">y</span> <span class="o">=</span> <span class="n">jax</span><span class="p">.</span><span class="n">pmap</span><span class="p">(</span><span class="k">lambda</span> <span class="n">v</span><span class="p">:</span> <span class="n">v</span> <span class="o">*</span> <span class="n">jax</span><span class="p">.</span><span class="n">process_index</span><span class="p">())(</span><span class="n">x</span><span class="p">)</span>
    <span class="k">print</span><span class="p">(</span><span class="s">"PMAP result:"</span><span class="p">,</span> <span class="n">y</span><span class="p">)</span>

<span class="n">client</span> <span class="o">=</span> <span class="n">TrainerClient</span><span class="p">()</span>
<span class="n">job_id</span> <span class="o">=</span> <span class="n">client</span><span class="p">.</span><span class="n">train</span><span class="p">(</span>
    <span class="n">runtime</span><span class="o">=</span><span class="s">"jax-distributed"</span><span class="p">,</span>
    <span class="n">trainer</span><span class="o">=</span><span class="n">CustomTrainer</span><span class="p">(</span><span class="n">func</span><span class="o">=</span><span class="n">get_jax_dist</span><span class="p">),</span>
<span class="p">)</span>
<span class="n">client</span><span class="p">.</span><span class="n">wait_for_job_status</span><span class="p">(</span><span class="n">job_id</span><span class="p">)</span>
<span class="k">print</span><span class="p">(</span><span class="s">"</span><span class="se">\n</span><span class="s">"</span><span class="p">.</span><span class="n">join</span><span class="p">(</span><span class="n">client</span><span class="p">.</span><span class="n">get_job_logs</span><span class="p">(</span><span class="n">name</span><span class="o">=</span><span class="n">job_id</span><span class="p">)))</span>
</code></pre></div></div>

<p>The <code class="language-plaintext highlighter-rouge">jax-distributed</code> runtime injects <code class="language-plaintext highlighter-rouge">JAX_NUM_PROCESSES, JAX_PROCESS_ID, and JAX_COORDINATOR_ADDRESS</code> into the environment, and all processes must call <code class="language-plaintext highlighter-rouge">jax.distributed.initialize()</code> exactly once before any JAX computation.</p>

<p>For more details, refer to the <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/jax/">Kubeflow Trainer JAX guide</a> for jax.distributed and SPMD primitives.</p>

<h2 id="bringing-xgboost-to-kubernetes-with-trainer">Bringing XGBoost to Kubernetes with Trainer</h2>

<p>Running distributed XGBoost workloads on Kubernetes has traditionally required manual setup of communication layers, environment variables, and cluster coordination. With this release, Kubeflow Trainer introduces built-in support for XGBoost, enabling seamless distributed training with minimal configuration.</p>

<p>The new xgboost-distributed runtime abstracts away the complexity of setting up XGBoost’s collective communication (Rabit). Trainer automatically provisions worker pods using JobSet and injects the required DMLC environment variables, allowing workers to coordinate and synchronize during training. The rank 0 pod is automatically configured to act as the tracker, simplifying cluster setup even further.</p>

<p>This integration supports both CPU and GPU workloads out of the box. For CPU training, each node runs a single worker leveraging OpenMP for intra-node parallelism. For GPU workloads, each GPU is mapped to an individual worker, enabling efficient scaling across nodes.</p>

<p>For more information, please see this <a href="https://github.com/kubeflow/trainer/blob/master/examples/xgboost/distributed-training/xgboost-distributed.ipynb">Notebook example</a> and <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/xgboost/">documentation guide</a>.</p>

<h2 id="track-trainjob-progress-and-expose-metrics">Track TrainJob Progress and Expose Metrics</h2>

<p>In this release, Kubeflow Trainer introduces a powerful new capability to automatically update TrainJob status with real-time training progress and metrics generated directly from your ML code. This enables key insights: such as percentage completion, estimated time remaining (ETA), and training metrics–to be surfaced through the TrainJob API, eliminating the need to manually inspect training logs.</p>

<h3 id="how-it-works">How it works</h3>

<p>When this feature is enabled (feature flag <code class="language-plaintext highlighter-rouge">TrainJobStatus</code> is required), Kubeflow Trainer starts an HTTP server that exposes endpoints for reporting training progress and metrics. Client applications can send updates to these endpoints, and the TrainJob controller will automatically reflect this information in the job status. Users can then easily access these insights through the Kubeflow SDK without needing to inspect logs.</p>

<p>To simplify adoption, we are collaborating with popular ML frameworks to integrate Kubeflow Trainer callbacks that automate this process. With these integrations, users don’t need to change anything to make it work!</p>

<p>For example, this functionality is already available in <a href="https://github.com/huggingface/transformers/issues/44486">Hugging Face Transformers</a>, where metrics are automatically reported when using the Trainer:</p>

<div class="language-py highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="kn">from</span> <span class="nn">transformers</span> <span class="kn">import</span> <span class="n">Trainer</span><span class="p">,</span> <span class="n">TrainingArguments</span>

<span class="n">trainer</span> <span class="o">=</span> <span class="n">Trainer</span><span class="p">(</span><span class="n">model</span><span class="o">=</span><span class="n">model</span><span class="p">,</span> <span class="n">args</span><span class="o">=</span><span class="n">TrainingArguments</span><span class="p">(...),</span> <span class="n">train_dataset</span><span class="o">=</span><span class="n">ds</span><span class="p">)</span>
<span class="n">trainer</span><span class="p">.</span><span class="n">train</span><span class="p">()</span>  <span class="c1"># Progress automatically reported when running in Kubeflow
</span></code></pre></div></div>

<h3 id="future-plans">Future Plans</h3>

<p>We have an exciting roadmap for this feature, including support for periodic, transparent checkpointing based on ETA, as well as integration with OptimizationJob for hyperparameter tuning jobs.</p>

<p>To learn more about this feature please see <a href="https://github.com/kubeflow/trainer/tree/master/docs/proposals/2779-trainjob-progress">this proposal.</a></p>

<h2 id="bringing-flux-framework-for-hpc-and-mpi-bootstrapping">Bringing Flux Framework for HPC and MPI Bootstrapping</h2>

<p>Setting up distributed ML training jobs using MPI can be very time consuming: from stitching together launcher-worker topologies to configuring SSH-based bootstrapping, there’s a lot of moving parts that require code on top of your training code. In v2.2, Kubeflow Trainer brings the Flux Framework – a workload manager that combines hierarchical job management with graph-based scheduling – to handle your HPC-style scheduling needs without the overhead that typically comes with it.</p>

<p>Flux uses ZeroMQ to bootstrap MPI, an improvement over traditional SSH, and also brings PMIx and support for more MPI variants. When a training job is submitted, an init container automatically handles Flux’s installation, meaning that you do not need to install Flux to your application container. The plugin also handles cluster discovery, broker configuration, and CURVE certificate generation to provide cryptographic security for the overlay network.</p>

<p>For teams whose workloads sit at the intersection of ML and HPC, Flux serves as a portability layer that enables running simulation alongside AI/ML workloads. Scheduling to Flux bypasses any potential etcd bottlenecks, and the limitations of the Kubernetes scheduler that require tricks to batch schedule to an underlying single-pod queue. Flux enables fine-grained control over where pods land, and is ideal when you are running simulation pipelines that feed into model Training. This integration also enables the use of Process Management Interface Exascale (PMIx) to manage and coordinate large-scale MPI workloads on Kubernetes using TrainJobs, something that was previously not possible.</p>

<p>Apply the Flux runtime and a TrainJob manifest. For example:</p>

<div class="language-shell highlighter-rouge"><div class="highlight"><pre class="highlight"><code>kubectl apply <span class="nt">--server-side</span> <span class="nt">-f</span> https://raw.githubusercontent.com/kubeflow/trainer/refs/heads/master/examples/flux/flux-runtime.yaml
kubectl apply <span class="nt">-f</span> https://raw.githubusercontent.com/kubeflow/trainer/refs/heads/master/examples/flux/lammps-train-job.yaml
</code></pre></div></div>

<p>After that, monitor the pods with <code class="language-plaintext highlighter-rouge">kubectl get pods --watch</code>, and inspect the lead broker logs with <code class="language-plaintext highlighter-rouge">kubectl logs &lt;pod-name&gt; -c node -f</code> . This also shows how to run the Flux cluster in interactive mode with <code class="language-plaintext highlighter-rouge">flux-interactive.yaml</code>, and then use <code class="language-plaintext highlighter-rouge">kubectl exec</code> and <code class="language-plaintext highlighter-rouge">flux proxy</code> to connect to the lead broker Flux instance and manually run LAMMPS inside the cluster.</p>

<p>The Flux runtime depends on the <code class="language-plaintext highlighter-rouge">mlPolicy: flux</code> trigger in flux-runtime.yaml, and you can customize the setup through environment variables such as <code class="language-plaintext highlighter-rouge">FLUX_VIEW_IMAGE and FLUX_NETWORK_DEVICE</code>. Binaries are installed under <code class="language-plaintext highlighter-rouge">/mnt/flux</code>, software is copied to <code class="language-plaintext highlighter-rouge">/opt/software</code>, and configurations are stored in <code class="language-plaintext highlighter-rouge">/etc/flux-config</code>. Related documentation includes the Kubeflow Trainer Getting Started guide, the Flux example manifests, and the Flux Framework HPSF project resources. A simple implementation has been done for this first go, and users are encouraged to submit feedback to request exposure of additional features. A demo video will be showcased at the KubeCon + CloudNativeCon 2026 EU booth for those that can attend.</p>

<p>You can learn more about this in our <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/flux/">Flux Guide</a>.</p>

<h2 id="resource-timeout-for-trainjobs">Resource Timeout for TrainJobs</h2>

<p>Previously, TrainJob resources persisted in the cluster indefinitely after completion unless manually removed, which led to Etcd bloat, resource contention and no automatic garbage collection. A job could also get stuck or run indefinitely, wasting CPU/GPU capacity and reducing cluster efficiency. In v2.2, Kubeflow Trainer adds support for ActiveDeadlineSeconds API in TrainJob. This field lets users set a hard timeout (in seconds) for a TrainJob’s active execution timeline. When the deadline is exceeded, Trainer marks the TrainJob as Failed (reason: <code class="language-plaintext highlighter-rouge">DeadlineExceeded</code>), terminates the running workload, and deletes the underlying JobSet.</p>

<p>There’s a couple ways to specify the timeout limit of a job, the first one is by modifying the TrainJob manifest directly:</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>apiVersion: trainer.kubeflow.org/v1alpha1
kind: TrainJob
metadata:
  name: quick-experiment
spec:
  activeDeadlineSeconds: 28800 #Max runtime 8 hours
runtimeRef:
  name: torch-distributed-gpu
trainer:
  image: my-training:latest
  numNodes: 2
</code></pre></div></div>

<p>More information about how to configure lifecycle policies for TrainJobs can be found in our <a href="https://www.kubeflow.org/docs/components/trainer/user-guides/trainjob-lifecycle/">TrainJob Lifecycle Guide</a></p>

<h2 id="runtimepatches-api-to-override-trainjob-defaults">RuntimePatches API to override TrainJob defaults</h2>

<p>In many distributed learning environments, multiple controllers can interact with the same TrainJob manifest, making ownership boundaries really important to preserve. The new RuntimePatches API replaces PodTemplateOverrides with a manager-keyed structure that makes it explicit on who applied what and when.</p>

<p>Each patch is scoped to a named manager and can target specific jobs or pods within the runtime, with both job-level and pod-level overrides supported. This means Kueue can inject node selectors and tolerations into the trainer pod without conflicting with another controller managing job-level metadata, and the full history of what was applied is preserved directly in the spec.</p>

<p>In the new TrainJob manifest, every manager owns its own entry, pod and job overrides are separate fields under that manager. Note that your manager field will be <strong>immutable</strong> after creation:</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>apiVersion: trainer.kubeflow.org/v2alpha1
kind: TrainJob
metadata:
  name: pytorch-distributed
spec:
  runtimeRef:
    name: pytorch-distributed-gpu
  trainer:
    image: docker.io/custom-training
  runtimePatches:
    - manager: trainer.kubeflow.org/kubeflow-sdk # who owns this entry (immutable)
      trainingRuntimeSpec:
        template:
          spec:
            replicatedJobs:
              - name: node
                template:
                  spec:
                    template:
                      spec:
                        nodeSelector:
                          accelerator: nvidia-tesla-v100
</code></pre></div></div>

<p>Note that the RuntimePatches API cannot be used to set environment variables for the node, dataset-initializer, or model-initializer containers, nor to override command, args, image, or resources on the trainer container.</p>

<p>For a complete description of the API’s structure, restrictions and use cases, check out the <a href="https://www.kubeflow.org/docs/components/trainer/operator-guides/runtime-patches/#runtimepatches-overview">RuntimePatches Operator Guide</a>.</p>

<p>⚠️ <strong>This API introduces Breaking Changes!!</strong></p>

<p>PodTemplateOverrides has been removed in v2.2. If you’re currently using it in your TrainJob manifests, you’ll need to migrate to the RuntimePatches API.</p>

<h2 id="breaking-changes">Breaking Changes</h2>

<p>This release introduces a set of architectural improvements and breaking changes that lay the foundations for a more scalable and modularized Trainer. Please review the following when upgrading to Trainer v2.2:</p>

<h3 id="replace-podtemplateoverrides-with-runtimepatches-api">Replace PodTemplateOverrides with RuntimePatches API</h3>

<p>As mentioned above, PodTemplateOverrides has been replaced with RuntimePatches API to support manager-scoped customization and prevent conflicts when multiple controllers are patching the same TrainJob.</p>

<p>If you are using PodTemplateOverrides in your TrainJob manifests or SDK code, you will need to migrate to the manager-keyed RuntimePatches structure. See the  <a href="https://www.kubeflow.org/docs/components/trainer/operator-guides/runtime-patches/#runtimepatches-overview">RuntimePatches Operator Guide</a>, and <a href="https://sdk.kubeflow.org/en/latest/train/options.html">Options Reference</a> for more information.</p>

<h3 id="remove-numprocpernode-from-the-torch-mlpolicy-api">Remove numProcPerNode from the Torch MLPolicy API</h3>

<p>The numProcPerNode field has been removed from the Torch MLPolicy. Process-per-node configuration is now handled directly through the container resources, so any TrainJob manifests or SDK calls that set numProcPerNode explicitly will need to be updated before upgrading to v2.2.</p>

<h3 id="remove-elasticpolicy-api">Remove ElasticPolicy API</h3>

<p>The ElasticPolicy API has been removed from MLPolicy in Trainer v2.2. Elastic training is not yet available in this release, we are actively working on a <a href="https://github.com/kubeflow/trainer/issues/2903">redesigned implementation</a> for future release. If your TrainJobs rely on elastic training configuration, please hold off on upgrading until that work lands.</p>

<h3 id="some-trainjob-api-fields-are-now-immutable">Some TrainJob API fields are now immutable</h3>

<p>Several TrainJob spec fields are now properly enforced as immutable after job creation. This rejects modifications to fields such as .spec.trainer.image on a running TrainJob upfront instead of having it silently fail at the JobSet controller level. If your workflows rely on updating these fields on a running TrainJob, those updates will now be rejected by the admission webhood. Please review your TrainJob update logic to ensure compatibility with our immutability policies in v2.2.</p>

<h2 id="release-notes">Release Notes</h2>

<p>For the complete list of all pull requests, visit the GitHub release page: https://github.com/kubeflow/trainer/releases/tag/v2.2.0</p>

<h2 id="roadmap-moving-forward">Roadmap Moving Forward</h2>

<p>We are excited to continue pushing Kubeflow as a state of the art platform for distributed ML training by making TrainJob manifests more observable and more performant across a wide range of hardware.</p>

<p>One area we’re particularly excited about is bringing Multi-Node NVLink (MNNVL) support for TrainJobs, 
enabling them to treat GPUs across multiple machines as a single unified memory domain. For 
large-scale training, this means significantly faster node-to-node communication compared to 
standard network-based primitives and brings forth a new era of configurations that simply 
weren’t practical before on Kubernetes. We are working closely with Kubernetes community to introduce first class support for Dynamic Resource Allocation (DRA) in TrainJobs.</p>

<p>We look forward to introducing Automatic configuration of GPU requests for TrainJobs that will
take the guesswork out of choosing the right resources. With intelligent methods guiding the
process, Trainer will choose appropriate resources automatically based on the TrainJob configuration.
This gives teams the power to plan experiments with confidence and trust that jobs use just the right
amount of compute.</p>

<p>Workload-Aware Scheduling (WAS) is also actively being integrated with the native Kubernetes Workload API for TrainJob to bring robust gang-scheduling support for distributed training without third party plugins. The integration will be available after Kubernetes v1.36, and we plan to extend it further to support Topology-Aware Scheduling and Dynamic Resource Allocation (DRA) as those APIs mature.</p>

<p>A full list of our 2026 roadmap can be found <a href="https://github.com/kubeflow/trainer/pull/3242">here</a>.</p>

<h2 id="join-the-community">Join the Community</h2>

<p>The Kubeflow Trainer is built by and for the community. We welcome contributions, feedback, and participation from everyone! We want to thank the community for their contributions to this release. We invite you to:</p>

<h3 id="contribute">Contribute:</h3>

<ul>
  <li>Read the <a href="https://github.com/kubeflow/trainer/blob/master/CONTRIBUTING.md">Contributing Guide</a>.</li>
  <li>Browse the <a href="https://github.com/kubeflow/trainer/issues?q=is%3Aissue%20state%3Aopen%20good%20first%20issues">good first issues</a></li>
  <li>Explore the <a href="https://github.com/kubeflow/trainer">GitHub Repository</a></li>
</ul>

<h3 id="connect-with-the-community">Connect with the Community:</h3>

<ul>
  <li>Join <a href="https://cloud-native.slack.com/archives/C0742LDFZ4K">#kubeflow-trainer</a> on <a href="https://www.kubeflow.org/docs/about/community/#kubeflow-slack-channels">CNCF Slack</a></li>
  <li>Attend our biweekly <a href="https://docs.google.com/document/d/1MChKfzrKAeFRtYqypFbMXL6ZIc_OgijjkvbqmwRV-64/edit?tab=t.0">Kubeflow Trainer and Katib meetings</a></li>
</ul>

<h3 id="learn-more">Learn More:</h3>

<ul>
  <li>View the <a href="https://github.com/kubeflow/trainer/releases/tag/v2.2.0">GitHub Release</a></li>
  <li>Explore the <a href="https://www.kubeflow.org/docs/components/trainer/">Kubeflow Trainer docs</a></li>
</ul>

<p><strong>Headed to <a href="https://events.linuxfoundation.org/kubecon-cloudnativecon-europe/">KubeCon + CloudNativeCon 2026 EU</a>?</strong> Stop by the Kubeflow booth to see these features in action 😸🧊!!</p>
