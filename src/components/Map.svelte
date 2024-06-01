<script>
  import { onMount } from "svelte";
  import mapboxgl from "mapbox-gl";
  import "mapbox-gl/dist/mapbox-gl.css";
  import bbox from "@turf/bbox";

  export let sites;
  export let activeSite = undefined;

  let map;

  let isLoaded = false;

  mapboxgl.accessToken = import.meta.env.VITE_MAPBOX_TOKEN;

  onMount(() => {
    map = new mapboxgl.Map({
      container: "map",
      style: "mapbox://styles/agwaterdesk/clwqjql88011601qe1sd89qte",
      interactive: false,
    });

    map.fitBounds([-95.526209, 28.809257, -87.572107, 46.19747], {
      duration: 0,
      padding: { bottom: 10 },
    });

    map.setMaxBounds(map.getBounds());

    map.on("load", () => {
      isLoaded = true;
      // Add the GeoJSON data as a new source
      map.addSource("sites", {
        type: "geojson",
        data: sites,
      });

      // Add a layer to visualize the features as circle markers
      map.addLayer(
        {
          id: "sites-markers",
          type: "circle",
          source: "sites",
          paint: {
            "circle-color": "#fff",
            "circle-radius": 5,
            "circle-stroke-width": 2,
            "circle-stroke-color": "#F1B82D",
          },
        },
        "settlement-subdivision-label"
      );

      // Add an event listener for the circle markers
      map.on("click", "sites-markers", function (e) {
        activeSite = e.features[0].properties.site_id;
      });

      // Change the cursor to a pointer when the mouse is over the sites-markers layer
      map.on("mouseenter", "sites-markers", function () {
        map.getCanvas().style.cursor = "pointer";
      });

      // Change it back to default when it leaves
      map.on("mouseleave", "sites-markers", function () {
        map.getCanvas().style.cursor = "";
      });
    });
  });

  $: if (activeSite) {
    map.setPaintProperty("sites-markers", "circle-stroke-color", [
      "case",
      ["==", ["get", "site_id"], activeSite],
      "#000000",
      "#F1B82D",
    ]);

    map.setPaintProperty("sites-markers", "circle-color", [
      "case",
      ["==", ["get", "site_id"], activeSite],
      "#F1B82D",
      "#fff",
    ]);
  } else {
    if (isLoaded) {
      map?.setPaintProperty("sites-markers", "circle-stroke-color", "#F1B82D");

      map?.setPaintProperty("sites-markers", "circle-color", "#fff");
    }
  }
</script>

<div>
  <div id="map"></div>
</div>

<style lang="scss">
  #map {
    width: 100%;
    height: 100%;
  }
</style>
