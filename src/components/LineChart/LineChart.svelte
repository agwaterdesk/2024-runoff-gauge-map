<script>
  import { LayerCake, Svg, Html, groupLonger, flatten } from 'layercake';

  import { scaleOrdinal } from 'd3-scale';
  import { timeParse, timeFormat } from 'd3-time-format';
  import { format } from 'd3-format';


  import MultiLine from './_components/MultiLine.svelte';
  import AxisX from './_components/AxisX.svelte';
  import AxisY from './_components/AxisY.svelte';


  export let data;
  export let xKey;
  // export let yKey;
  export let label;
  export let color;

  const yKey = 'value';

  const zKey = 'measure';

  const xKeyCast = timeParse('%Y-%m-%d');

  const seriesNames = ['TONS', 'RollingAvg']
  const seriesColors = ['#6da34d', '#f1b82d'];

  /* --------------------------------------------
   * Cast values
   */
  data.forEach(d => {
    d[xKey] = typeof d[xKey] === 'string'
      ? xKeyCast(d[xKey])
      : d[xKey];

    seriesNames.forEach(name => {
      d[name] = +d[name];
    });
  });


  const groupedData = groupLonger(data, seriesNames, {
    groupTo: zKey,
    valueTo: yKey
  });

  function abbreviateNumber(number) {
    if (number >= 1e9) {
      return (number / 1e9).toFixed(1).replace(/\.0$/, "") + "B";
    } else if (number >= 1e6) {
      return (number / 1e6).toFixed(1).replace(/\.0$/, "") + "M";
    } else if (number >= 1e3) {
      return (number / 1e3).toFixed(1).replace(/\.0$/, "") + "k";
    } else {
      return number.toFixed(0).toString();
    }
  }


</script>



<div class="chart-hed">{label}</div>
<div class="chart-dek">In tons</div>

<div class="chart-container">
  <LayerCake
    padding={{ top: 7, right: 10, bottom: 20, left: 25 }}
    x={xKey}
    y={yKey}
    z={zKey}
    yDomain={[0, null]}
    zScale={scaleOrdinal()}
    zRange={seriesColors}
    flatData={flatten(groupedData, 'values')}
    data={groupedData}
  >
    <Svg>
      <AxisX gridlines={false} ticks={4}/>
      <AxisY ticks={4} format={(d) => abbreviateNumber(d)} />
      <MultiLine/>
    </Svg>
  </LayerCake>
</div>

<style lang="scss">

  .chart-container {
    width: 100%;
    height: 200px;
    padding: 0.5rem;
  }

  .chart-hed {
    @include font-size(14px);
    font-weight: bold;
    color: $color-body-text-light;
  }

  .chart-dek {
    @include font-size(14px);
    font-style: italic;
    color: $color-body-text-light;
  }
</style>
