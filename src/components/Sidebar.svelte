<script>
  import { slide } from "svelte/transition";

  import data from "../data/data.json";
  import SidebarLegend from "./SidebarLegend.svelte";
  import LineChart from "./LineChart/LineChart.svelte";

  export let sites;
  export let activeSite;

  let sidebarHeight;
  let activeSiteData;

  let gap = 8;
  $: cellHeight = sidebarHeight / sites.features.length - gap;

  function handleClick(siteId) {
    if (activeSite == siteId) {
      activeSite = undefined;
      return;
    }
    activeSite = siteId;
  }

  $: if (activeSite) {
    activeSiteData = data.filter((d) => d.SITE_QW_ID == activeSite);
  }
</script>

<div
  id="sidebar"
  bind:clientHeight={sidebarHeight}
  style:--gap="{gap}px"
  class:expanded={Boolean(activeSite)}
>
  <div class="scroll-wrapper">
    {#each sites.features as site}
      <div
        id="site-{site.properties.site_id}"
        style:--cell-height="{cellHeight}px"
        class="site"
        class:active={site.properties.site_id == activeSite}
        class:inactive={activeSite && site.properties.site_id != activeSite}
        on:click={() => handleClick(site.properties.site_id)}
        on:keydown={() => handleClick(site.properties.site_id)}
      >
        <h3>{site.properties.site_name}</h3>

        {#if site.properties.site_id == activeSite}
          <div class="g-drawer" in:slide={{ axis: "y" }}>
            <SidebarLegend />
            <LineChart
              data={activeSiteData.filter((d) => d.CONSTIT == "TN")}
              xKey="WY"
              yKey="TONS"
              label="Total nitrogen"
              color="#6DA34D"
            />

            <LineChart
              data={activeSiteData.filter((d) => d.CONSTIT == "TP")}
              xKey="WY"
              yKey="TONS"
              label="Total phosphorus"
              color="#548687"
            />
          </div>
        {/if}
      </div>
    {/each}
  </div>
</div>

<style lang="scss">
  #sidebar {
    height: 100%;
    width: 100%;
    &.expanded {
      overflow-y: scroll;
    }

    .scroll-wrapper {
      display: flex;
      flex-direction: column;
      gap: var(--gap);
    }

    .site {
      background: #f8f8f8;
      width: 100%;
      height: var(--cell-height);
      border-radius: 3px;
      border: 1px solid #ddd;
      border-left: 5px solid #f1b82d;
      padding: 0.25rem 0.5rem;
      flex-grow: 1;
      display: flex;
      flex-direction: column;
      justify-content: center;
      font-weight: 500;
      cursor: pointer;

      h3 {
        color: #555;
      }

      &:hover,
      &.active {
        h3 {
          color: #000;
          margin-bottom: 0.125rem;
          font-weight: 700;
        }
      }

      &.active {
        height: unset;

        .g-drawer {
        }
      }

      &.inactive {
        opacity: 0.6;
      }
    }
  }
</style>
