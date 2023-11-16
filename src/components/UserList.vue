<template>
  <div class="user-list">
    <div
      class="user-card cursor-pointer shadow mb-3 bg-white rounded"
      v-for="user in users"
      :key="user.login.uuid"
      @click="showOnMap(user)"
    >
      <div class="card border-0">
        <div class="card-title mb-0 d-flex align-items-center">
          <img
            class="m-2 rounded-circle shadow-sm border"
            :width="50"
            :height="50"
            :src="user.picture.large"
          />
          <div>
            <h5 class="m-0">{{ user.name.first }} {{ user.name.last }}</h5>
            <span class="card-text">{{ user.email }}</span>
          </div>
        </div>
        <div class="card-body pt-0">
          <div class="card-text">
            <p class="mb-2">
              <strong>Gender: </strong> <span>{{ user.gender }}</span>
            </p>
            <p class="mb-2">
              <strong>Country: </strong>
              <span
                >{{ user.location.country }} - {{ user.location.city }} -
                {{ user.location.street.name }}</span
              >
            </p>
            <p class="mb-0">
              <strong>DOB: </strong>
              <span>{{ user.dob.date | formatDate }}</span>
            </p>
          </div>
        </div>
      </div>
    </div>
    <!-- Next button to load more users -->
    <button
      class="btn btn-sm btn-primary mb-5 w-100"
      @click="getNextUsers"
      v-if="users.length >= 5"
    >
      Load More
    </button>
  </div>
</template>

<script>
export default {
  props: {
    users: Array,
    showOnMap: Function,
    getNextUsers: Function
  },
  filters: {
    formatDate (value) {
      const date = new Date(value)
      return date.toLocaleDateString()
    }
  }
}
</script>
<style>
.cursor-pointer {
  cursor: pointer;
}
.card-text {
  font-size: 12px;
}
</style>
