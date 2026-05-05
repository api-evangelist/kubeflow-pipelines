---
title: "Modernizing Kubeflow Pipelines UI"
url: "https://blog.kubeflow.org/modernizing-kubeflow-pipelines-ui/"
date: "2026-03-31T00:00:00-05:00"
author: "Manaswini Das"
feed_url: "https://blog.kubeflow.org/feed.xml"
---
<p>The Kubeflow Pipelines web interface has been upgraded from React 16 to React 19 — a modernization effort that touches every layer of the frontend stack. Whether you use the UI to manage pipelines day-to-day or contribute to the codebase, here is what this means for you.</p>

<h2 id="whats-changing-for-users">What’s changing for users</h2>

<p>You do not need to do anything differently. Your bookmarks, workflows, and browser all work exactly as before. But under the hood, the UI is now built on a modern foundation that delivers tangible improvements:</p>

<h3 id="a-faster-more-responsive-interface">A faster, more responsive interface</h3>

<p>React 18 introduced automatic batching, which reduces unnecessary re-renders across the UI. In practice, this means pages like Run Details, Experiment Details, and the pipeline creation flow respond faster to your interactions. Forms validate without flicker, and multi-step workflows feel snappier. The production bundle size stayed exactly the same — 0% increase — so page load times are unchanged.</p>

<h3 id="smoother-pipeline-graph-navigation">Smoother pipeline graph navigation</h3>

<p>The pipeline DAG visualization (the graph you see when inspecting a pipeline’s structure) has been migrated from the deprecated react-flow-renderer to @xyflow/react. This brings improved pan, zoom, and drag performance, especially on larger or more complex pipeline graphs. If you’ve ever experienced sluggishness when navigating a deeply nested pipeline, this upgrade directly addresses that.</p>

<h3 id="improved-charts-and-metrics-display">Improved charts and metrics display</h3>

<p>Run metrics and comparison charts now use Recharts instead of the deprecated react-vis library. The new charting library renders more efficiently, handles edge cases better, and provides cleaner visual output when comparing run results side by side.</p>

<h3 id="better-accessibility">Better accessibility</h3>

<p>The component library migration from Material-UI v3 to MUI v5 brings improved keyboard navigation, better ARIA attribute coverage, and more consistent focus management across dialogs, tables, and form elements. These improvements make the UI more usable with screen readers and keyboard-only workflows.</p>

<h3 id="no-breaking-changes">No breaking changes</h3>

<p>Every user-facing feature works the same way it did before. The API contracts are unchanged. If you use the KFP Python SDK or REST API to interact with the platform, nothing changes on your end. This upgrade was purely a frontend modernization — zero impact on backend behavior, pipeline execution, or artifact storage.</p>

<h2 id="why-we-made-this-change">Why we made this change</h2>

<p>The KFP frontend had been running on React 16 (released in 2017) with Material-UI v3, create-react-app, and Jest/Enzyme for testing. This created compounding issues:</p>

<ul>
  <li><strong>Security exposure.</strong> React 16 and 17 no longer receive security patches, and dozens of transitive dependencies were locked to outdated versions because of React peer constraints.</li>
  <li><strong>Stalled ecosystem.</strong> Modern libraries — including improved data-fetching, visualization, and accessibility tools — dropped support for React 16/17. Staying behind meant the UI could not benefit from upstream improvements.</li>
  <li><strong>Contributor friction.</strong> The legacy CRA + Jest + Enzyme toolchain was slow to build, brittle to test, and increasingly difficult for new contributors to set up. Modernizing the stack lowers the barrier to contribution.</li>
</ul>

<h2 id="how-we-got-here">How we got here</h2>

<p>Rather than attempting a single risky version jump, we followed a deps-first, bump-last strategy: upgrade every dependency to be forward-compatible before touching React itself. A custom React peer compatibility gate in CI prevented regressions at every step. The work was executed across <strong>20+</strong> pull requests in strict dependency order.</p>

<h3 id="react-16--17-rebuilding-the-foundation">React 16 → 17: Rebuilding the foundation</h3>

<p>Before React could move forward, the entire build and test toolchain had to be replaced. create-react-app was swapped for Vite, Jest + Enzyme gave way to Vitest + Testing Library, and Material-UI was upgraded from v3 to v4 to unblock the React 17 peer range. The deprecated react-vis charting library was replaced with Recharts. With those blockers cleared, the React 17 bump itself was a small, low-risk change.</p>

<h3 id="react-17--18-the-biggest-leap">React 17 → 18: The biggest leap</h3>

<p>This phase required the most dependency work. Storybook jumped from v6 straight to v10 on the Vite builder. Material-UI v4 was migrated to MUI v5 with Emotion. react-query moved to @tanstack/react-query v4. react-flow-renderer was replaced with @xyflow/react. After all ecosystem deps cleared the peer gate, the React 18 core bump landed — followed by careful stabilization of automatic batching behavior in class components that were reading stale state.</p>

<h3 id="react-18--19-the-final-stretch">React 18 → 19: The final stretch</h3>

<p>A deprecation audit at React 18.3 found zero React-specific warnings. A final dependency sweep cleared the last peer blockers (react-ace, transitive react-redux). The React 19 bump resolved the final allowlist entry and handled a small set of API changes like the removal of forwardRef in test mocks.</p>

<h2 id="the-full-stack-transformation">The full stack transformation</h2>

<p>Over the course of this effort, virtually every layer of the frontend stack was modernized:</p>

<table>
  <thead>
    <tr>
      <th>Layer</th>
      <th>Before</th>
      <th>After</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>React</td>
      <td>16</td>
      <td>19</td>
    </tr>
    <tr>
      <td>Build system</td>
      <td>Create React App + Craco</td>
      <td>Vite</td>
    </tr>
    <tr>
      <td>Test framework</td>
      <td>Jest + Enzyme</td>
      <td>Vitest + Testing Library</td>
    </tr>
    <tr>
      <td>UI component library</td>
      <td>Material-UI v3</td>
      <td>MUI v5 + Emotion</td>
    </tr>
    <tr>
      <td>Data fetching</td>
      <td>react-query v3</td>
      <td>@tanstack/react-query v4</td>
    </tr>
    <tr>
      <td>Pipeline graph</td>
      <td>react-flow-renderer v9</td>
      <td>@xyflow/react</td>
    </tr>
    <tr>
      <td>Charts</td>
      <td>react-vis</td>
      <td>Recharts</td>
    </tr>
    <tr>
      <td>Storybook</td>
      <td>6 (Webpack)</td>
      <td>10 (Vite)</td>
    </tr>
  </tbody>
</table>

<h2 id="by-the-numbers">By the numbers</h2>

<ul>
  <li>20+ PRs merged across the entire React 16-to-19 effort</li>
  <li><strong>15 tracked milestones</strong> executed in strict dependency order</li>
  <li><strong>0% bundle size increase</strong> — page load times unchanged</li>
  <li><strong>0 React deprecation warnings</strong> at the 18.3 checkpoint audit</li>
  <li><strong>0 breaking changes</strong> to user-facing features or APIs</li>
</ul>

<h2 id="want-to-contribute">Want to contribute?</h2>

<p>The full execution plan with every PR, issue, and dependency graph is tracked in the <a href="https://github.com/kubeflow/pipelines/blob/master/frontend/docs/react-18-19-upgrade-checklist.md">react-18-19-upgrade-checklist.md</a>. Look for miscellaneous bugs, report bugs, help with reviews and help improve our documentation.</p>

<p>Huge thanks to <a href="https://github.com/jeffspahr">@jeffspahr</a>, <a href="https://github.com/kanishka-commits">@kanishka-commits</a>, <a href="https://github.com/PR3MM">@PR3MM</a>, <a href="https://github.com/jsonmp-k8">@jsonmp-k8</a>, <a href="https://github.com/dpanshug">@dpanshug</a>, and <a href="https://github.com/rishi-jat">@rishi-jat</a> for contributing to this effort and reviewing all the contributions leading up to this milestone!</p>
