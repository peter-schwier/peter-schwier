---
title: XState Docs
pageTitle: XState Docs
length: 5612
author: 
timestamp: 2023-04-16T15:04:13-05:00
markdownload-tags: []
markdownload-source: https://xstate.js.org/docs/
markdownload-hostname: xstate.js.org
---

# XState Docs

## Excerpt
> Documentation for XState: State Machines and Statecharts for the Modern Web

---
 [![XState logotype][fig1]  
<sub><strong>JavaScript state machines and statecharts</strong></sub>](https://xstate.js.org/)

 [![npm version][fig2] (opens new window)](https://badge.fury.io/js/xstate) ![][fig3]

JavaScript and TypeScript [finite state machines (opens new window)](https://en.wikipedia.org/wiki/Finite-state_machine) and [statecharts (opens new window)](https://www.sciencedirect.com/science/article/pii/0167642387900359/pdf) for the modern web.

📖 [Read the documentation (opens new window)](https://stately.ai/docs/xstate)

💙 [Explore our catalogue of examples (opens new window)](https://xstate-catalogue.com/)

➡️ [Create state machines with the Stately Editor (opens new window)](https://stately.ai/editor)

🖥 [Download our VS Code extension (opens new window)](https://marketplace.visualstudio.com/items?itemName=statelyai.stately-vscode)

📑 Adheres to the [SCXML specification (opens new window)](https://www.w3.org/TR/scxml/)

💬 Chat on the [Stately Discord Community (opens new window)](https://discord.gg/xstate)

## [#](https://xstate.js.org/docs/#packages) Packages

-   🤖 `xstate` - Core finite state machine and statecharts library + interpreter
-   [🔬 `@xstate/fsm` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-fsm) - Minimal finite state machine library
-   [📉 `@xstate/graph` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-graph) - Graph traversal utilities for XState
-   [⚛️ `@xstate/react` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-react) - React hooks and utilities for using XState in React applications
-   [💚 `@xstate/vue` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-vue) - Vue composition functions and utilities for using XState in Vue applications
-   [🎷 `@xstate/svelte` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-svelte) - Svelte utilities for using XState in Svelte applications
-   [🥏 `@xstate/solid` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-solid) - Solid hooks and utilities for using XState in Solid applications
-   [✅ `@xstate/test` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-test) - Model-Based-Testing utilities (using XState) for testing any software
-   [🔍 `@xstate/inspect` (opens new window)](https://github.com/statelyai/xstate/tree/main/packages/xstate-inspect) - Inspection utilities for XState

## [#](https://xstate.js.org/docs/#templates) Templates

Get started by forking one of these templates on CodeSandbox:

-   [XState Template (opens new window)](https://codesandbox.io/s/xstate-example-template-m4ckv) - no framework
-   [XState + TypeScript Template (opens new window)](https://codesandbox.io/s/xstate-typescript-template-s9kz8) - no framework
-   [XState + React Template (opens new window)](https://codesandbox.io/s/xstate-react-template-3t2tg)
-   [XState + React + TypeScript Template (opens new window)](https://codesandbox.io/s/xstate-react-typescript-template-wjdvn)
-   [XState + Vue Template (opens new window)](https://codesandbox.io/s/xstate-vue-template-composition-api-1n23l)
-   [XState + Vue 3 Template (opens new window)](https://codesandbox.io/s/xstate-vue-3-template-vrkk9)
-   [XState + Svelte Template (opens new window)](https://codesandbox.io/s/xstate-svelte-template-jflv1)

## [#](https://xstate.js.org/docs/#super-quick-start) Super quick start

## [#](https://xstate.js.org/docs/#promise-example) Promise example

[📉 See the visualization on stately.ai/viz (opens new window)](https://stately.ai/viz?gist=bbcb4379b36edea0458f597e5eec2f91)

See the code

-   [Visualizer](https://xstate.js.org/docs/#visualizer)
-   [Why?](https://xstate.js.org/docs/#why)
-   [Finite State Machines](https://xstate.js.org/docs/#finite-state-machines)
-   [Hierarchical (Nested) State Machines](https://xstate.js.org/docs/#hierarchical-nested-state-machines)
-   [Parallel State Machines](https://xstate.js.org/docs/#parallel-state-machines)
-   [History States](https://xstate.js.org/docs/#history-states)

## [#](https://xstate.js.org/docs/#visualizer) Visualizer

**[Visualize, simulate, inspect, and share your statecharts in XState Viz (opens new window)](https://stately.ai/viz)**

[![XState Viz][fig4]](https://stately.ai/viz "XState Viz")

**[stately.ai/viz (opens new window)](https://stately.ai/viz)**

## [#](https://xstate.js.org/docs/#why) Why?

Statecharts are a formalism for modeling stateful, reactive systems. This is useful for declaratively describing the _behavior_ of your application, from the individual components to the overall application logic.

Read [📽 the slides (opens new window)](http://slides.com/davidkhourshid/finite-state-machines) ([🎥 video (opens new window)](https://www.youtube.com/watch?v=VU1NKX6Qkxc)) or check out these resources for learning about the importance of finite state machines and statecharts in user interfaces:

-   [Statecharts - A Visual Formalism for Complex Systems (opens new window)](https://www.sciencedirect.com/science/article/pii/0167642387900359/pdf) by David Harel
-   [The World of Statecharts (opens new window)](https://statecharts.github.io/) by Erik Mogensen
-   [Pure UI (opens new window)](https://rauchg.com/2015/pure-ui) by Guillermo Rauch
-   [Pure UI Control (opens new window)](https://medium.com/@asolove/pure-ui-control-ac8d1be97a8d) by Adam Solove
-   [Spectrum - Statecharts Community (opens new window)](https://spectrum.chat/statecharts) (For XState specific questions, please use the [GitHub Discussions (opens new window)](https://github.com/statelyai/xstate/discussions))

## [#](https://xstate.js.org/docs/#finite-state-machines) Finite State Machines

[![Finite states][fig5]  
Open in Stately Viz](https://stately.ai/viz/2ac5915f-789a-493f-86d3-a8ec079773f4 "Finite states")

## [#](https://xstate.js.org/docs/#hierarchical-nested-state-machines) Hierarchical (Nested) State Machines

[![Hierarchical states][fig6]  
Open in Stately Viz](https://stately.ai/viz/d3aeda4f-7f8e-44df-bdf9-dd3cdafb3312 "Hierarchical states")

**Object notation for hierarchical states:**

## [#](https://xstate.js.org/docs/#parallel-state-machines) Parallel State Machines

[![Parallel states][fig7]  
Open in Stately Viz](https://stately.ai/viz/9eb9c189-254d-4c87-827a-fab0c2f71508 "Parallel states")

## [#](https://xstate.js.org/docs/#history-states) History States

[![History state][fig8]  
Open in Stately Viz](https://stately.ai/viz/33fd92e1-f9e6-49e6-bdeb-cef9e60195ec "History states")

Special thanks to the sponsors who support this open-source project:

[![Transloadit logo][fig9]  
Transloadit](https://transloadit.com/)

## [#](https://xstate.js.org/docs/#semver-policy) SemVer Policy

We understand the importance of the public contract and do not intend to release any breaking changes to the **runtime** API in a minor or patch release. We consider this with any changes we make to the XState libraries and aim to minimize their effects on existing users.

### [#](https://xstate.js.org/docs/#breaking-changes) Breaking changes

XState executes much of the user logic itself. Therefore, almost any change to its behavior might be considered a breaking change. We recognize this as a potential problem but believe that treating every change as a breaking change is not practical. We do our best to implement new features thoughtfully to enable our users to implement their logic in a better, safer way.

Any change _could_ affect how existing XState machines behave if those machines are using particular configurations. We do not introduce behavior changes on a whim and aim to avoid making changes that affect most existing machines. But we reserve the right to make _some_ behavior changes in minor releases. Our best judgment of the situation will always dictate such changes. Please always read our release notes before deciding to upgrade.

### [#](https://xstate.js.org/docs/#typescript-changes) TypeScript changes

We also reserve a similar right to adjust declared TypeScript definitions or drop support for older versions of TypeScript in a minor release. The TypeScript language itself evolves quickly and often introduces breaking changes in its minor releases. Our team is also continuously learning how to leverage TypeScript more effectively - and the types improve as a result.

For these reasons, it is impractical for our team to be bound by decisions taken when an older version of TypeScript was its latest version or when we didn’t know how to declare our types in a better way. We won’t introduce declaration changes often - but we are more likely to do so than with runtime changes.

### [#](https://xstate.js.org/docs/#packages-2) Packages

Most of the packages in the XState family declare a peer dependency on XState itself. We’ll be cautious about maintaining compatibility with already-released packages when releasing a new version of XState, **but** each release of packages depending on XState will always adjust the declared peer dependency range to include the latest version of XState. For example, you should always be able to update `xstate` without `@xstate/react`. But when you update `@xstate/react`, we highly recommend updating `xstate` too.

Last Updated: 4/3/2023, 12:56:32 PM

[fig1]: https://raw.githubusercontent.com/statelyai/public-assets/main/logos/xstate-logo-black-nobg.svg
[fig2]: https://badge.fury.io/js/xstate.svg
[fig3]: https://opencollective.com/xstate/tiers/backer/badge.svg?label=sponsors&color=brightgreen
[fig4]: https://user-images.githubusercontent.com/1093738/131729181-5db835fc-77e7-4740-b03f-46bd0093baa1.png
[fig5]: https://user-images.githubusercontent.com/1093738/131727631-916d28a7-1a40-45ed-8636-c0c0fc1c3889.gif
[fig6]: https://user-images.githubusercontent.com/1093738/131727794-86b63c76-5ee0-4d73-b84c-6992a1f0814e.gif
[fig7]: https://user-images.githubusercontent.com/1093738/131727915-23da4b4b-5e7e-46ea-9c56-5093e37e60e6.gif
[fig8]: https://user-images.githubusercontent.com/1093738/131728111-819cc824-9881-4ecf-948a-00c1162cd9e9.gif
[fig9]: https://assets.transloadit.com/assets/images/logo-v2.svg

> Saved from https://xstate.js.org/docs/ on 2023-04-16T15:04:13-05:00