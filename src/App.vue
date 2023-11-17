<template>
  <div id="app">
    <nav
      class="navbar navbar-expand-sm bg-light navbar-light mb-5 p-4 d-flex justify-content-center"
    >
      <h3 class="text-center">Users List on Google Map</h3>
    </nav>
    <div class="userlist-map d-flex justify-content-center sm:d-block">
      <!-- User List Component -->
      <UserList
        :users="users"
        @nextUsersLoaded="loadNextUsers"
        @userClicked="showOnMap"
      />
      <!-- Google Map Component -->
      <MapView
        :markers="markers"
        :mapCenter="mapCenter"
        :mapZoom="mapZoom"
        @markerOnClicked="centerMap"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import UserList from "./components/UserList.vue";
import MapView from "./components/MapView.vue";

export default {
  components: {
    UserList,
    MapView
  },
  data() {
    return {
      users: [], // array to store all users
      mapCenter: { lat: 0, lng: 0 }, // initial map center
      mapZoom: 5, // initial map zoom level
      markers: []
    };
  },
  mounted() {
    this.getUsers();
  },
  methods: {
    getUsers() {
      axios
        .get("https://randomuser.me/api/?results=5")
        .then(response => {
          this.users = response.data.results;
          this.updateMap();
        })
        .catch(error => {
          console.error("Error fetching users:", error);
        });
    },
    loadNextUsers(newUsers) {
      this.users = this.users.concat(newUsers);
      this.updateMap();
    },
    updateMap() {
      this.markers = this.users.map(user => ({
        id: user.id,
        position: {
          lat: parseFloat(user.location.coordinates.latitude),
          lng: parseFloat(user.location.coordinates.longitude)
        },
        label: user.name.first
      }));
      this.mapCenter =
        this.markers.length > 0 ? this.markers[0].position : { lat: 0, lng: 0 };
    },
    showOnMap(user) {
      const userMarker = {
        id: user.id,
        position: {
          lat: parseFloat(user.location.coordinates.latitude),
          lng: parseFloat(user.location.coordinates.longitude)
        },
        label: `${user.name.first} ${user.name.last}`
      };
      this.markers = [userMarker];
      this.mapCenter = userMarker.position;
    },
    centerMap(marker) {
      this.mapCenter = marker.position;
    }
  }
};
</script>
<style>
@media (max-width: 767px) {
  .userlist-map {
    flex-direction: column;
  }
  .user-list {
    margin: 0 2rem;
  }
  .gmap-container {
    width: 90%;
  }
}
</style>
