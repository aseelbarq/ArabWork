<template>
  <div class="gmap-container">
    <GmapMap
      :center="{ lat: mapCenter.lat, lng: mapCenter.lng }"
      :zoom="mapZoom"
      style="width: 100%; height: 500px;"
    >
      <GmapMarker
        v-for="marker in markers"
        :key="marker.id"
        :position="marker.position"
        :clickable="true"
        @click="centerMap(marker)"
      />
    </GmapMap>
  </div>
</template>

<script>
export default {
  props: {
    markers: Array,
    mapCenter: Object,
    mapZoom: Number,
    centerMap: Function
  }
}
</script>

<style>
.gmap-container {
  width: 40%;
  margin: 0 2rem;
}
@media (max-width: 767px) {
  .gmap-container {
    width: 90%;
  }
}
</style>
