---
title: "KubeCon India 2025 with Kubeflow: Our Community Experience"
url: "https://blog.kubeflow.org/kubecon/community/2025/08/23/kubecon-2025-india-kubeflow.html"
date: "2025-08-23T00:00:00-05:00"
author: "Akash Jaiswal, Yash Pal"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<h2 id="introduction">Introduction</h2>

<p><img alt="KubeCon India 2025" src="https://blog.kubeflow.org/images/2025-08-23-kubecon-2025-india-kubeflow/KubeConIndiaKeynote.png" /></p>

<p>KubeCon + CloudNativeCon India 2025 in Hyderabad was an absolute blast! As a second-time attendee (<a href="https://github.com/jaiakash">Akash Jaiswal</a>) and a first-time attendee (<a href="https://github.com/yashpal2104">Yash Pal</a>), we couldn’t help but be blown away by the incredible energy at one of world’s biggest cloud native gatherings. We were super excited seeing Kubeflow get a special shoutout during the opening keynote for its role in cloud native AI/ML and MLOps - definitely made us proud to be part of the community! (Above image shows the keynote moment)</p>

<p>We also got super lucky with the chance to volunteer at the Kubeflow booth this year. We also met <a href="https://github.com/johnugeorge">Johnu George</a> in person, who delivered two amazing talks on Kubeflow’s latest capabilities. It was really exciting to finally meet community members face-to-face whom we’ve only seen in community calls and Slack!</p>

<p>This blog shares all the exciting bits from our packed 2 days at KubeCon - from awesome booth conversations to technical deep-dives. We hope this motivates more community members to not just contribute but also attend and help Kubeflow at events like KubeCon. Trust me, you won’t want to miss the next one! 😊</p>

<h2 id="featured-talks">Featured Talks</h2>

<ul>
  <li><strong>Cloud Native GenAI using KServe and OPEA</strong>
<strong>Speakers:</strong> <a href="https://github.com/johnugeorge">Johnu George</a>, Gavrish Prabhu (Nutanix)
<strong>Sched Link:</strong> <a href="https://kccncind2025.sched.com/event/23EtS/cloud-native-genai-using-kserve-and-opea-johnu-george-gavrish-prabhu-nutanix">View on Sched</a></li>
</ul>



<ul>
  <li><strong>Bridging Big Data and Machine Learning Ecosystems</strong>
<strong>Speakers:</strong> <a href="https://github.com/johnugeorge">Johnu George</a>, Shiv Jha (Nutanix)
<strong>Sched Link:</strong> <a href="https://kccncind2025.sched.com/event/23Eur/bridging-big-data-and-machine-learning-ecosystems-a-cloud-native-approach-using-kubeflow-johnu-george-shiv-jha-nutanix">View on Sched</a></li>
</ul>



<h2 id="kubeflow-booth-highlights">Kubeflow Booth Highlights</h2>

<p><img alt="Kubeflow Booth" src="https://blog.kubeflow.org/images/2025-08-23-kubecon-2025-india-kubeflow/KubeflowBoothPic.png" /></p>

<p>Here’s a picture of our Kubeflow booth volunteer team. It was really great to meet and interact with audiences who had dozens of questions about Kubeflow, contributors who wanted to help, and developers who were already using it and shared their experiences.</p>

<p>Here are some key highlights from our booth conversations:</p>

<ul>
  <li><strong>Community Engagement:</strong>
    <ul>
      <li>Discussions on real-world use cases and deployment strategies. Few users shared their experience of using Kubeflow in their companies and how its benefiting them.</li>
      <li>Many of the audience wants to learn more about how to explore and contribute to Kubeflow. (Answers: Join community calls, and check out GitHub for open issues)</li>
      <li>Several companies expressed interest in adopting projects like Kubeflow. Few senior engineers were already using it for some of their workloads, now they want to use it for production workload.</li>
    </ul>
  </li>
  <li><strong>Popular Questions from Audience:</strong>
    <ul>
      <li>How does Kubeflow simplify ML workflows using Kubernetes? Can you clarify why Kubeflow is not multicluster agnostic?
Answer: You can just send your jobs to 5 different and independent Kubeflow clusters if you want to. So We do not think that this is needed at all. We offer APIs for external access (KFP, everything you can also do in the UI) so we do not need the Kubeflow deployment to span multiple clusters directly. If you want to span multiple regions then either use the API of multiple independent Kubeflow clusters in different regions and just submit your jobs or use a Kubernetes layer that transparently handles clusters spanning multiple regions. But nevertheless adding this complexity burden on Kubeflow does not offer much benefit.</li>
      <li>How does Kubeflow integrate with other cloud-native tools? How is Kubeflow different from other tools in the industry?</li>
      <li>What are the security considerations for running ML pipelines? How can Kubeflow help optimize costs when working with LLMs, especially in terms of minimizing GPU usage to stay within quota limits while still delivering performance?</li>
      <li>How mature is Kubeflow today, and how well does it align with the workflows of different MLOps? What is the timeline of graduation for Kubeflow? What does the roadmap for Kubeflow look like?</li>
      <li>Why has Kubeflow chosen to integrate with ArgoCD rather than Tekton CD — the question that came up from a maintainer of the Tekton project.</li>
    </ul>
  </li>
</ul>

<h2 id="our-experience">Our experience</h2>

<p>What an incredible journey these past two days have been! Beyond the technical talks and booth duties, what really stood out was the genuine excitement around Kubeflow in the community. Seeing users’ faces light up when sharing their success stories, or watching newcomers get that “aha!” moment during demos - these are the moments that make community events special.</p>

<p>The technical discussions were mind-blowing too. From hearing how startups are using Kubeflow to train their LLMs, to learning how enterprises are scaling it across thousands of models - each conversation taught us something new. We even got into some heated (but friendly!) debates about MLOps architectures and the future of AI on Kubernetes.</p>

<p>But the best part? The people. Meeting community members we’ve only known through Slack emojis and GitHub comments to sharing chai/biryani with fellow contributors. These personal connections are what make the open source community truly special. Can’t wait for the next one! 🚀</p>

<h2 id="want-to-help">Want to help?</h2>

<p>The Kubeflow community holds open meetings and is always looking for more volunteers and users to unlock the potential of machine learning. If you’re interested in becoming a Kubeflow contributor, please feel free to check out the resources below. We look forward to working with you!</p>

<ul>
  <li>Visit our <a href="https://www.kubeflow.org/docs/about/community/">website</a> or <a href="https://github.com/kubeflow">GitHub</a> page.</li>
  <li>Join the <a href="https://www.kubeflow.org/docs/about/community/">Kubeflow Slack channels</a>.</li>
  <li>Join the <a href="https://groups.google.com/g/kubeflow-discuss">kubeflow-discuss</a> mailing list.</li>
  <li>Want to volunteer for such events, Join the <a href="https://cloud-native.slack.com/archives/C078ZMRQPB6">kubeflow-outreach</a> channel on CNCF Slack.</li>
  <li>Attend our weekly <a href="https://www.kubeflow.org/docs/about/community/#kubeflow-community-call">community meeting</a>.</li>
</ul>

<p>Feel free to share your thoughts or questions in the comments!</p>
