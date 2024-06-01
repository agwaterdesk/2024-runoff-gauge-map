<script>
  import Window from "./components/Window.svelte";
  import Map from "./components/Map.svelte";
  import Legend from "./components/Legend.svelte";
  import Sidebar from "./components/Sidebar.svelte";
  import { MousePointer } from "lucide-svelte";
  import sites from "./data/sites.json";

  // Handle responsive iframes for embeds
  import pym from "pym.js";

  new pym.Child({
    polling: 500,
  });

  function getUrlParameter(name) {
    const params = new URLSearchParams(window.location.search);
    return params.get(name);
  }

  let includeCredit = getUrlParameter("credit") != "false";

  let overlayInfo;
  let activeSite;
  let marker = null;

  $: if (activeSite) {
    setTimeout(() => {
      const element = document.getElementById(`site-${activeSite}`);
      element.scrollIntoView({ behavior: "smooth" });
    }, 250);
  }
</script>

<Window />
<!-- Outer div must have class 'chart-container' don't change -->
<div class="chart-container">
  <h1 class="headline">
    Select water-quality monitoring sites along the Mississippi River
  </h1>
  <p class="dek hide-in-static">
    <MousePointer size="14" />Click on a site to see its historic nitrogen and
    phosphorus trends
  </p>
  <p class="sr-only">
    A map showing select water-quality monitoring sites along the Mississippi
    River. The map displays the river's course from Minnesota to Louisiana, with
    highlighted sites marked by orange dots.
  </p>
  <Legend />

  <div id="g-viz">
    <Map {sites} bind:activeSite />
    <Sidebar {sites} bind:activeSite />
  </div>

  {#if includeCredit}
    <div class="credit">
      Data: <a target="_blank" href="https://nrtwq.usgs.gov/nwqn/#/GULF">USGS</a
      >; Graphic by Jared Whalen /
      <a target="_blank" href="https://agwaterdesk.org/">Ag & Water Desk</a>
    </div>
  {/if}
</div>

<style lang="scss">
  .chart-container {
    max-width: 800px;
    width: 100%;
    padding: 1rem;
  }

  #g-viz {
    position: relative;
    display: grid;
    gap: 8px;
    grid-template-columns: 1fr 1fr;
    overflow-y: hidden;
    height: 600px;
    @include mq(600px, "max-width") {
      height: 600px;
    }
  }

  .dek {
    display: flex;
    align-items: center;
  }
</style>
