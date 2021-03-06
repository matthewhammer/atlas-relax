# atlas-relax

Relax is a minimal, powerful declarative VDOM and reactive programming library.

[![Travis](https://img.shields.io/travis/atlassubbed/atlas-relax.svg)](https://travis-ci.org/atlassubbed/atlas-relax)

[<img align="right" width="250" height="250" src="https://user-images.githubusercontent.com/38592371/54162407-be63ba00-442b-11e9-9577-02fbf627d1f6.gif">](https://en.wikipedia.org/wiki/Atomic_orbital?q=into+the+rabbit+hole)

### just relax 😌

This tiny 2.5KB (.min.gz) engine lets you define data-driven apps using declarative components. Relax combines ideas from Meteor, Mithril and Preact into one simple library. Relax supports:

  * **Sideways** reactive data between components done right
    * reactive computations
    * reactive variables
    * reactive providers
  * Managed diffs (take imperative control when automatic diffs are too expensive)
    * **rebasing** work (add or override work in a diff)
    * **coherent** batched updates
    * **decoherent** time-sliced updates (incremental rendering)
  * **Fragments** and array return values from `render`
  * **Fibers** for efficiently tracking work that is being redone
  * **Scheduling** (async and sync)
  * Keyed JSX (e.g. `<li key="key1">`) for efficient diffing
  * Lifecycle methods/hooks
  * Memoization as a replacement for `shouldComponentUpdate`
  * Rendering-agnostic apps
    * render apps to arbitrary targets (not just the DOM)
  * Well-established DAG algorithms to ensure updates remain efficient (O(n)) and correct
    * stack-safe
    * atomic, solves the "diamond problem" (no redundant renders)

Relax gives you what you need to build not only simple todo apps, but also rapidly-updating apps like stock tickers.

### demos

  * [VDOM diffing visualized with force-directed graphs](https://github.com/atlassubbed/play-relax-visualized)
  * ["Fluid" rendering priority visualized](https://github.com/atlassubbed/play-fluid-priority) 
  * [Dynamic-interval component](https://github.com/atlassubbed/play-dynamic-polling)

### FAQs 🤔

  1. **Is JSX necessary? No.**
  2. **Is Relax a view library? Yes.** You can code a declarative tree and use a [DOM-rendering plugin](https://github.com/atlassubbed/atlas-mini-dom) to render it to the DOM, or a String-rendering plugin to render it to HTML.
  3. **Is Relax a state management library? Yes.** Relax's state management primitives are powerful enough that you could implement your own MOBX/Redux/`Meteor.Tracker.autorun` in [a few lines of code](https://github.com/atlassubbed/atlas-munchlax) on top of Relax.
  4. **Do I need Redux or MOBX? No.** Relax's reactive primitives are sufficient for all apps.
  5. **Do I need something like React hooks? No.** Sufficient lifecycle methods are provided. If you prefer hooks (closures), you could implement React hooks on top of Relax's lifecycle methods in a few lines of code.
  6. **Do updates cause the whole app to re-render? No.** Updates scale linearly with the radius of the update, not with the total graph size. If an update only affects 5 nodes, then only those 5 nodes will get their `render` called.
  7. **Do re-renders always update the DOM? No.** Mutations are calculated with a keyed diffing algorithm to limit interactions with the DOM. Plugins (DOM Renderer, SSR Renderer, etc.) don't have to think -- Relax "tells plugins what to do".
  8. **Are there docs and demos I can read? Soon.** I'm currently working on all of that stuff.

### build your own X 

Relax isn't a "framework", it's a small library that helps you avoid huge frameworks. Relax abstracts out the heavy lifting associated with common UI and state management tasks (reconciliation, efficient data flow propagation, reactive functions, etc.). **Build your own framework**: Many frameworks can be built in a few lines of code with Relax's primitives:
  
  * [React DOM](https://github.com/atlassubbed/atlas-mini-dom) (as a plugin)
  * [MOBX](https://github.com/atlassubbed/atlas-munchlax)
  * Redux

Relax's inner-diff (instance-level `diff`) API is inspired by Mithril's `redraw`. If you prefer React syntax, hooks and other React-like APIs can also be built pretty easily with Relax's primitives:

  * `setState`
  * functional `setState`
  * `useEffect`
  * `useLayoutEffect`
  * `useState`
  * `useRef`
  * Sky is the limit: `afterAll`, `after`, `before`, `beforeAll`, `afterMount`, `beforeUnmount`, `afterUpdate`

If you've ever tinkered with Meteor, you've probably been obsessed with `Tracker.autorun` at some point. Nobody blames you -- it is awesome. If you wanted to, you could implement the exact same API using Relax nodes as Relax supports dependency graphs out-of-the-box. Relax makes it easy to implement reactive patterns such as:
  
  * [`Meteor.Tracker`](https://github.com/atlassubbed/atlas-munchlax)
  * [`Meteor.ReactiveVar`](https://github.com/atlassubbed/atlas-munchlax)
  * `Meteor.ReactiveDict`
  * `Meteor.Collection` (reactive collection)
  * Efficient `Meteor.Collection.find` that supports sort, filter, and multiple listeners per query

### install

```
npm install --save atlas-relax
```

You are assumed to be familiar with transpiling code (babel, webpack, etc.). In the future, I may make a `dist/` folder containing various transpiled-minified code for quick installment.

### notes

Relax is an experimental library and your feedback is greatly appreciated! If Relax has piqued your interest 👀 then read the source code! I've included implementation comments for your reference. If you are curious how some things were implemented, check out the [development history](https://github.com/atlassubbed/history-atlas-relax). There are a few things I want to implement in the future:

  1. Make plugins agnostic to reducible nodes (makes DOM rendering more efficient)
  2. Error boundaries -- they're useful for larger apps.
     * not as easy to implement as in React, since we may generalize it to DAGs
  3. Ensure DOM renderers can properly hydrate an existing tree to decrease time-to-mount
  4. Docs, demos and examples 
     * [Basic DOM Renderer](https://github.com/atlassubbed/atlas-mini-dom) (plugin) in 20 lines of code
     * Functional [reactive framework in 20 lines of code](https://github.com/atlassubbed/atlas-munchlax)
  5. Re-roll code into modules, provide optimized `dist/` files
     * Use rollup, babel and terser to generate ready-to-use distribution payloads
  6. Reverse queued managed mounts so they mount in expected order.

### inspired by 💜

Meteor, Mithril, Preact and physics analogies. MIT License.

The gif above was made with the help of Paul Falstad's [atomic dipole transition applet](http://www.falstad.com/qmatomrad/). Check out his other amazing interactive physics playgrounds over at his website: [http://www.falstad.com/mathphysics.html](http://www.falstad.com/mathphysics.html).
