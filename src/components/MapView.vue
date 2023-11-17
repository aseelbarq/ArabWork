<template>
  <div class="gmap-container">
    <GmapMap
      :center="{ lat: mapCenter.lat, lng: mapCenter.lng }"
      :zoom="mapZoom"
      style="width: 100%; height: 500px;"
    >
      <GmapMarker
      v-for="(marker,index) in markers"
        :key="marker.index"
        :position="marker.position"
        :clickable="true"
        @click="handleMarker(marker)"
        :label="marker.label"
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
  },
  data() {
    return {
      selectedMarkerId: null
    };
  },
  methods: {
    handleMarker(marker) {
      this.$emit('markerOnClicked', marker);
    }
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
