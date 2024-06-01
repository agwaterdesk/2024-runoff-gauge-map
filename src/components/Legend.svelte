<script>
  import BasinSvg from "./BasinSvg.svelte";
</script>

<div class="legend">
  <div class="items">
    <div class="legend-item">
      <div class="gauge"></div>
      <span>National Water Quality Network site</span>
    </div>
    <div class="legend-item">
     <BasinSvg width=25 fill="#B0B3B3"/>
      <span>Mississippi/Atchafalaya River Basin</span>
    </div>
  </div>
</div>

<style>
  .legend {
    margin-bottom: 0.5rem;
  }

  .items {
    display: flex;
    align-items: center;
    gap: 1rem;
  }
  .legend-item {
    display: flex;
    align-items: center;
    gap: 0.15rem;
  }
  .legend-item span {
    font-size: 0.875rem;
    color: #656568;
  }

  .gauge {
    width: 12px;
    height: 12px;
    border: 2px solid #f1b82d;
    border-radius: 100%;
  }
</style>
