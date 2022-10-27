---
# Global Config
theme: default
highlighter: shiki
titleTemplate: '%s'
lineNumbers: false
drawings:
  persist: false
css: unocss

# Slide Config
title: Location, Origin, ContentWindow
layout: cover
background: https://images.unsplash.com/photo-1516156008625-3a9d6067fab5?auto=format&fit=crop&w=1920&monochrome=4af
class: text-center
---
# ~~Location, Location, Location~~
# `location`, `origin`, `contentWindow`

An exercise in coding through real estate bureaucracy

<div class="abs-br m-6 flex gap-2">
  <Button @click="$slidev.nav.next" />
</div>

<style>
/* Make it pretty */
.slidev-layout h1 {
  &:first-child { margin-bottom: 0 }
  code { @apply text-4xl text-teal-500 dark:text-teal-400 }
  code:nth-child(2) { @apply text-orange-500 dark:text-orange-400 }
  code:nth-child(3) { @apply text-rose-500 dark:text-rose-400 }
  + p { @apply text-orange-300  opacity-100 text-shadow }
}
</style>
<!--
  - Self-intro ~1m
  - it's a story of love
  - and discovery
  - and a little improvising
-->

---
layout: two-cols
clicks: 10
class: m-auto
---

# The Client

<v-click>

MGS Group
</v-click>

<v-clicks at="3">

- Luxury Real Estate in Boston
- Low volume, high value
- Old site is slow
- Familiar & Comfortable in WordPress
</v-clicks>
<v-clicks at="8">

- Fragmented listings experience
</v-clicks>

::right::

# The Ask
<v-click at="2">

New Site Build
</v-click>

<v-clicks at="3">

- Beautiful (already designed! 🎨)
- More dynamic & seamless UX (Vue 💚)
- Faster (Edge caching 🏎💨)
</v-clicks>
<v-clicks at="7">

- ...keep WordPress 🤷‍♂️

</v-clicks>
<v-clicks at="9">

- <span :class="$slidev.nav.clicks > 9 ? 'text-orange-500 dark:text-orange-400 animate-pulse' : ''">Unified MLS Listings & Search</span>
</v-clicks>

<!-- <div
  v-if="$slidev.nav.currentPage === 7"
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }">
  Slidev
</div> -->

---
title: Context
layout: intro
---

## Quick Rundown on MLS

<v-clicks>

**Easy Stuff: Terminology**
- **MLS** (Multiple Listings Service) - geographically-bound organization that maintains a database of real estate listings
- **Realtor**<sup>&reg;</sup> - a real estate broker and member of the National Association of Realtors (**NAR**)
- **IDX** (internet data exchange) - a data syndication agreement granted by an MLS, managed by the NAR

**Hard Mode: Our Main Characters**
</v-clicks>

<v-clicks>

- **MGS Group** LLC - The client in question, a real estate brokerage firm operating primarily the Boston area
- **MLS PIN** Inc. - The MLS representing Massachusetts and other parts of New England
- **IDX Broker** LLC - service that acts as a middleman for MLSes, allowing real estate firms to display listings to end users without the overhead of licensing and data management
</v-clicks>

<style>
  li b, li strong { @apply text-orange-500 }
</style>


---
class: text-center
---

## Who, What, and Where


<div class="mt-12" v-click>

```mermaid {securityLevel: 'loose', scale: 0.8, flowchart: { nodeSpacing: 1 }, themeCSS: '.cluster-label .nodeLabel { position: relative; top: -5px }' }
flowchart
    %% agt["fas:fa-user-tie\n Agent/Realtor®"]
    %% mgs["fas:fa-building\n Brokerage Firm \n (MGS Group)"]
    %% idx["fas:fa-server\nData Provider\n (IDX Broker LLC)"]
    %% mls[("fas:fa-list\n Listing Service \n (MLS PIN Inc.)")]
    %% ext["fas:fa-share-alt\n Aggregate Services\n (Zillow)"]

    agt["Agent/Broker/Realtor"]
    mgs["MGS Group"]
    idx["Middleman Service\n (IDX Broker LLC)"]
    mls["Listing Service \n (MLS PIN)"]
    ext["Aggregate Services (Zillow)"]


    agt-. work for .-> mgs
    mgs-. act as .->agt

    agt-- author listings -->mls


    mls-->idx

    mgs-->idx
    idx-->mgs


    mls-->ext
    idx-->ext

```
</div>


---
class: text-center
clicks: 4
---

## I thought this was about JavaScript APIs?

<div class="mt-1/5" v-if="$slidev.nav.clicks === 1">

  ### Oh..
</div>

<div class="mt-1/5" v-if="$slidev.nav.clicks === 2">

  ### yeah let's get back to that
</div>

<v-click at="3">

```mermaid {securityLevel: 'loose', scale: 0.7, flowchart: { nodeSpacing: 1 }, themeCSS: '.cluster-label .nodeLabel { position: relative; top: -5px }' }
flowchart
    %% agt["fas:fa-user-tie\n Agent/Realtor®"]
    %% mgs["fas:fa-building\n Brokerage Firm \n (MGS Group)"]
    %% idx["fas:fa-server\nData Provider\n (IDX Broker LLC)"]
    %% mls[("fas:fa-list\n Listing Service \n (MLS PIN Inc.)")]
    %% ext["fas:fa-share-alt\n Aggregate Services\n (Zillow)"]

    agt["Agent/Broker/Realtor"]
    mgs["MGS Group"]
    idx["Middleman Service\n (IDX Broker LLC)"]
    mls[("Listing Service \n (MLS PIN)")]
    ext["Aggregate Services (Zillow)"]


    agt<-.-> mgs

    agt-- author listings -->mls


    mls-->idx

    idx-- authored\n listing data -->mgs
    mgs-- supplement data\n for authored listings -->idx


    mls-->ext
    idx-->ext

    subgraph etc[ ]
        agt
        mls
        ext
    end
    style etc fill:none,stroke:none,stroke-width:2px

    subgraph dev[Developer Purview\n]
        mgs
        idx
    end
    style dev fill:none,stroke:#f66,stroke-width:2px,stroke-dasharray: 5 5,color:#f66

```
</v-click>


---

# Let's fetch some MLS listings!

<div class="grid grid-cols-3 gap-2 text-sm" style="--slidev-code-font-size: 8px">
  <div v-click>

  Utopian GraphQL?

  ```vue
  <script setup>
  import { useQuery } from '@vue/apollo-composable'
  import gql from 'graphql-tag'

  const { result } = useQuery(gql`
    query getListings {
      listings {
        id
        title
        status
        price
      }
    }
  `)
  </script>
  ```
  </div>
  <div v-click>

  Mundane JSON API?

  ```vue
  <script setup>
  const listings = reactive({
      data: [],
      filteredData: computed(() => processData()),
    });
  const fetchListings = async () => {
      axios
        .get("https://api.idxbroker.com/mls/api/v2/boston")
        .then((response) => {
          listings.data = response.data.results
        });
    };
  </script>
  ```
  </div>
  <div v-click>

  Undocumented XML Schemas?

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <!DOCTYPE RateResponse SYSTEM "https://api.idxbroker.com/exec/mls/MlsService.dtd"
  ><RateResponse> <Status>OK</Status> <Order sequence="1">
  <Listings><Listing method="PUBLIC"> <City>Boston</City>
  <Status isValid="YES" active="YES" PPX="NO">On Sale</Status>
  <MlsCode>PMFS</MlsCode> <Cost currency="USD" converted="NO"
  originalCurrency="USD" originalCost="3999542.90">3999542.90</Cost> </Listing>
  <Listing method="PUBLIC"> <City>Cambridge</City>
  <Status isValid="YES" active="YES" PPX="NO">On Sale</Status>
  <MlsCode>PMFS</MlsCode> <Cost currency="USD" converted="NO"
  originalCurrency="USD" originalCost="74828274.90">74828274.90</Cost> </Listing>
  ```
  </div>
</div>


---

# Nothing.


<div class="mt-20 w-1/3 mx-auto">

```js
/**


  TODO 🎉


*/
```
</div>

---
layout: image-right
image: https://images.unsplash.com/photo-1566397000179-d83e7535349d?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2150&q=80
---

# IDX Usage Restrictions

<v-clicks>

1. MGS does not have the right to forward/proxy IDX feeds
2. NAR also explicitly forbids web scraping (except search engines)
3. IDX Broker does not provide a complete API

</v-clicks>

<div class="mt-16" v-click>

### Where there's a will...
</div>

---
layout: image-right
image: https://images.unsplash.com/photo-1604265497895-9ff77ad70e7a?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1287&q=80
---

# There's iFrame 🖼
(sorry)

<v-click>

Still the most popular way to display MLS listings on the web:
</v-click>

<v-clicks>

- ready-built solutions
- minimal technical overhead
- compatible with most sites to some degree
- limits ecosystem fragmentation
</v-clicks>

---
layout: image-right
image: https://images.unsplash.com/photo-1587929359039-e1b1dfe314df?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3432&q=80
---

# Downsides/Challenges

(in brief)

<v-clicks>

Impact on SEO

</v-clicks>
<v-clicks>

  - content within an `iframe` aren't attributed to the host container
  - improper use of meta tags between `iframe` and host can lead to SEO disaster
</v-clicks>

<v-clicks>

Impact on UX
</v-clicks>
<v-clicks>

- ready-built solutions break visual continuity
- scrollbars in `window` and `iframe` viewports will confuse users
- gaps in history & state tracking
</v-clicks>


---
layout: two-cols
---

# The Result
The component that was promised
<v-click>

```vue {all|15}
<!-- pages/idx/_.vue -->

<template>
  <header>
    <SearchBar />
    <DynamicPageHeader />
  </header>

  <!-- iFrame goes here -->
  <IdxFrame />

</template>

<script setup>
const idxFrame = useIdxFrameMessageLayer(frame)
const { url, pageType, heading } = useIdxFrameCtx(idxFrame)
</script>
```
</v-click>

::right::

<div class="mt-24" v-click>

#### `useIdxFrameMessageLayer()`
Custom communication layer for Vue-bound iFrames
</div>
<v-clicks>

- utilizes the `contentWindow.postMessage()` API
- watches for `message` events
- emits the iframe's current URL, loading status, scroll height, and other page data, 
- attempts to gracefully mask timeouts, errors, and invalid states
</v-clicks>

---

# Going Deeper
<v-click>

```js {monaco} {maxHeight:10}
// somewhere in the iframe's script tags

const root = window.self === window.top
document.documentElement.classList.add('app-' + root ? 'root' : 'frame')
function frameSetup () {
  // 1. send initial data burst to parent
  window.parent.postMessage(initialData, '*')

  // 2. Use Proxy for reporting changes with Vue-esque reactivity model
  const state = new Proxy({ size, url, status },  proxyReportingConfig)

  // 3. Create message event handler to allow execution of some actions
  const actions = {
    'css:inject': css => appendCss(css, true),
    'stylesheet:inject': src => appendCss(src, false),
  }

  // 4. Live document size updates with ResizeObserver
  const sizeWatcher = new ResizeObserver((entries) => {}

  // 5. Fixing IDX-injected content
  fixInsecureLinks() // force all links to use HTTPS
  fixPageActions()   // route form submissions to HTTPS
  purifySrcs()       // remove certain redundant libraries

  // 6. Page-specific data reporting
  getIdxPageInfo()

  // 7. Proactive page navigation reporting
  document.addEventListener('click', (e) => {})
}
```
</v-click>


---

# Bonus goodies
- Custom build system for the iframe HTML wrapper

  ```json
  {
    "scripts": {
      "build:idx": "NUXT_ENV_IDX=true NUXT_HOST='https://domain.com' nuxt generate --target='static'",
    }
  }
  ```
- Advanced page type prediction using query parameter and past states
- Custom search bar that routes to different IDX search endpoints
- WordPress integration for featured properties
- WPGraphQL abstraction layer page content
- Custom REST endpoints too!