<!--
  @component
  Generates an SVG multi-series line chart. It expects your data to be an array of objects, each with a `values` key that is an array of data objects.
 -->
<script>
  import { getContext } from "svelte";
  import { line, curveBasis } from "d3-shape";

  const { data, xGet, yGet, zGet } = getContext("LayerCake");

  /** @type {Function} [curve=curveLinear] - An optional D3 interpolation function. See [d3-shape](https://github.com/d3/d3-shape#curves) for options. Pass this function in uncalled, i.e. without the open-close parentheses. */
  export let curve = curveBasis;

  $: path = line()
    .x($xGet)
    .y($yGet)
    .curve(curve)
    .defined((d) => !!d.value); // Exclude y values of 0
</script>

<g class="line-group">
  {#each $data as group}
    <path class="path-line" d={path(group.values)} stroke={$zGet(group)}></path>
    <circle
      r="5"
      stroke={$zGet(group)}
      stroke-width="3"
      fill="white"
      cx={$xGet(group.values[group.values.length - 1])}
      cy={$yGet(group.values[group.values.length - 1])}
    ></circle>
  {/each}
</g>

<style>
  .path-line {
    fill: none;
    stroke-linejoin: round;
    stroke-linecap: round;
    stroke-width: 3px;
  }
</style>
