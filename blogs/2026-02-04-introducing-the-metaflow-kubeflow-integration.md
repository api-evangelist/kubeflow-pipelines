---
title: "Introducing the Metaflow-Kubeflow Integration"
url: "https://blog.kubeflow.org/metaflow/"
date: "2026-02-04T00:00:00-06:00"
author: "{\"name\"=>\"\", \"email\"=>\"\"}"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<h1 id="a-tale-of-two-flows-metaflow-and-kubeflow">A tale of two flows: Metaflow and Kubeflow</h1>

<p>Metaflow is a Python framework for building and operating ML and AI projects, originally developed and open-sourced by Netflix in 2019. In many ways, Kubeflow and Metaflow are cousins: closely related in spirit, but designed with distinct goals and priorities.</p>

<p><a href="https://docs.metaflow.org/">Metaflow</a> emerged from Netflix’s need to empower data scientists and ML/AI developers with developer-friendly, Python-native tooling, so that they could easily iterate quickly on ideas, compare modeling approaches, and ship the best solutions to production without heavy engineering or DevOps involvement. On the infrastructure side, Metaflow started with AWS-native services like AWS Batch and Step Functions, later expanding to provide first-class support for the Kubernetes ecosystem and other hyperscaler clouds.</p>

<p>In contrast, Kubeflow began as a set of Kubernetes operators for distributed TensorFlow and Jupyter Notebook management. Over time, it has evolved into a comprehensive Cloud Native AI ecosystem, offering a broad set of tools out of the box. These include Trainer, Katib, Spark Operator for orchestrating distributed AI workloads, Workspaces for interactive development environments, Hub for AI catalog and artifacts management, KServe for model serving, and Pipelines to deploy end-to-end ML workflows and stitching Kubeflow components together.</p>

<p>Over the years, Metaflow has delighted end users with its intuitive APIs, while Kubeflow has delivered tons of value to infrastructure teams through its robust platform components. This complementary nature of the tools motivated us to build a bridge between the two: <a href="https://docs.metaflow.org/production/scheduling-metaflow-flows/scheduling-with-kubeflow">you can now author projects in Metaflow and deploy them as Kubeflow Pipelines</a>, side by side with your existing Kubeflow workloads.</p>

<h1 id="why-metaflow--kubeflow">Why Metaflow → Kubeflow</h1>

<p>In <a href="https://www.cncf.io/wp-content/uploads/2025/11/cncf_report_techradar_111025a.pdf">the most recent CNCF Technology Radar survey</a> from October 2025, Metaflow got the highest positive scores in the “<em>likelihood to recommend</em>” and “<em>usefulness</em>” categories, reflecting its success in providing a set of stable, productivity-boosting APIs for ML/AI developers.</p>

<p>Metaflow spans the entire development lifecycle—from early experimentation to production deployment and ongoing operations. To give you an idea, the core features below illustrate the breadth of its API surface, grouped by project stage:</p>

<h2 id="development">Development</h2>

<ul>
  <li>
    <p>Straightforward APIs for <a href="https://docs.metaflow.org/metaflow/basics">creating and composing workflows</a>.</p>
  </li>
  <li>
    <p>Automated state transfer and management through <a href="https://docs.metaflow.org/metaflow/basics#artifacts">artifacts</a>, allowing you to <a href="https://docs.metaflow.org/metaflow/authoring-flows/introduction">build flows incrementally</a> and resume them freely (see <a href="https://netflixtechblog.com/supercharging-the-ml-and-ai-development-experience-at-netflix-b2d5b95c63eb">a recent article by Netflix</a> about the topic)</p>
  </li>
  <li>
    <p>Interactive, <a href="https://docs.metaflow.org/metaflow/basics">real-time visual outputs</a> from tasks through cards - a perfect substrate for <a href="https://outerbounds.com/blog/visualize-everything-with-ai">custom observability solutions, created quickly with AI copilots</a>.</p>
  </li>
  <li>
    <p>Choose the right balance between code and configuration through <a href="https://docs.metaflow.org/metaflow/configuring-flows/introduction">built-in configuration management</a>.</p>
  </li>
  <li>
    <p>Create domain-specific abstractions and project-level policies through <a href="https://docs.metaflow.org/metaflow/composing-flows/introduction">custom decorators</a>.</p>
  </li>
</ul>

<h2 id="scaling">Scaling</h2>

<ul>
  <li>
    <p><a href="https://docs.metaflow.org/scaling/remote-tasks/introduction">Scale flows horizontally and vertically</a>: Both task and data parallelism are supported.</p>
  </li>
  <li>
    <p><a href="https://docs.metaflow.org/scaling/failures">Handle failures gracefully</a>.</p>
  </li>
  <li>
    <p><a href="https://docs.metaflow.org/scaling/dependencies">Package dependencies automatically</a> with support for Conda, PyPI, and uv.</p>
  </li>
  <li>
    <p>Leverage <a href="https://docs.metaflow.org/scaling/remote-tasks/distributed-computing">distributed computing paradigms</a> such as Ray, MPI, and Torch Distributed.</p>
  </li>
  <li>
    <p><a href="https://docs.metaflow.org/scaling/checkpoint/introduction">Checkpoint long-running tasks</a> and manage checkpoints consistently.</p>
  </li>
</ul>

<h2 id="deployment">Deployment</h2>

<ul>
  <li>
    <p>Maintain a clear separation between experimentation, production, and individual developers through <a href="https://docs.metaflow.org/scaling/tagging">namespaces</a>.</p>
  </li>
  <li>
    <p>Adopt CI/CD and GitOps best practices through <a href="https://docs.metaflow.org/production/coordinating-larger-metaflow-projects">branching</a>.</p>
  </li>
  <li>
    <p><a href="https://docs.metaflow.org/production/event-triggering">Compose large, reactive systems</a> through isolated sub-flows with event triggering.</p>
  </li>
</ul>

<p>These features provide a unified, user-facing API for the capabilities required by real-world ML and AI systems. Behind the scenes, Metaflow is built on integrations with production-quality infrastructure, effectively acting as a user-interface layer over platforms like Kubernetes - and now, Kubeflow. The diagram below illustrates the division of responsibilities:
<img alt="kubeflow-metaflow-arch" src="https://github.com/user-attachments/assets/88f4af4e-7e27-4287-b275-88e4b1b87449" style="height: auto; display: block;" /></p>

<p>The key benefit of the Metaflow–Kubeflow integration is that it allows organizations to <strong>keep their existing Kubernetes and Kubeflow infrastructure intact, while upgrading the developer experience with higher-level abstractions and additional functionality, provided by Metaflow.</strong></p>

<p>Currently, the integration supports deploying Metaflow flows as Kubeflow Pipelines. Once you have Metaflow tasks running on Kubernetes, you can access other components such as Katib and Trainer from Metaflow tasks through their Python clients as usual.</p>

<h1 id="metaflow--kubeflow-in-practice">Metaflow → Kubeflow in practice</h1>

<p>As the integration requires no changes in your existing Kubeflow infrastructure, it is straightforward to get started. You can <a href="https://docs.metaflow.org/getting-started/infrastructure">deploy Metaflow in an existing cloud account</a> (GCP, Azure, or AWS) or you can <a href="https://docs.metaflow.org/getting-started/devstack">install the dev stack on your laptop</a> with a single command.</p>

<p>Once you have Metaflow and Kubeflow running independently, you can install the extension providing the integration (you can <a href="https://docs.metaflow.org/production/scheduling-metaflow-flows/scheduling-with-kubeflow">follow instructions in the documentation</a>):</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>pip install metaflow-kubeflow
</code></pre></div></div>

<p>The only configuration needed is to point Metaflow at your Kubeflow Pipelines service, either by adding the following line in the Metaflow config or by setting it as an environment variable:</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>METAFLOW_KUBEFLOW_PIPELINES_URL = "http://my-kubeflow"
</code></pre></div></div>

<p>After this, you can author a Metaflow flow as usual and test it locally:</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>python flow.py run
</code></pre></div></div>

<p>which runs the flow quickly as local processes. If everything looks good, you can deploy the flow as a Kubeflow pipeline:</p>

<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>python flow.py kubeflow-pipelines create
</code></pre></div></div>

<p>This will package all the source code and dependencies of the flow automatically, compile the Metaflow flow into a Kubeflow Pipelines YAML and deploy it to Kubeflow, which you can see alongside your existing pipelines in the Kubeflow UI. The following screencast shows the process in action:</p>

<p><a href="https://www.youtube.com/watch?v=ALg0A9SzRG8"><img alt="" src="https://i.ytimg.com/vi/ALg0A9SzRG8/maxresdefault.jpg" /></a></p>

<p>The integration doesn’t have 100% feature coverage yet: Some Metaflow features such as <a href="https://docs.metaflow.org/metaflow/basics#conditionals">conditional</a> and <a href="https://docs.metaflow.org/metaflow/basics#recursion">recursive</a> steps are not yet supported. In future versions, we may also provide additional convenience APIs for other Kubeflow components, such as KServe - or you can easily implement them by yourself as <a href="https://docs.metaflow.org/metaflow/composing-flows/custom-decorators">custom decorators</a> with the <a href="https://sdk.kubeflow.org/en/latest/">Kubeflow SDK</a>!</p>

<p>If you want to learn more about the integration, you can watch <a href="https://www.youtube.com/watch?v=YDKRIiQNMU0">an announcement webinar</a> on Youtube.</p>

<h1 id="feedback-welcome">Feedback welcome!</h1>

<p>Like Kubeflow, Metaflow is an open-source project actively developed by multiple organizations — including Netflix, which maintains a dedicated team working on Metaflow, and <a href="https://outerbounds.com">Outerbounds, which provides a managed Metaflow platform</a> deployed in customers’ own cloud environments.</p>

<p>The Metaflow community convenes at <a href="http://slack.outerbounds.co">the Metaflow Slack</a>. We welcome you to join, ask questions, and give feedback about the Kubeflow integration, and share your wishlist items for the roadmap. We are looking forward to a fruitful collaboration between the two communities!</p>
