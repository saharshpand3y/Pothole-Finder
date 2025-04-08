<template>
  <div class="dashboard">
    <div class="table-container">
      <PotholeTable :potholes="potholes" @pothole-deleted="fetchPotholes" />
    </div>
    <div class="map-container">
      <GoogleMap :potholes="potholes" />
    </div>
  </div>
</template>

<script>
import PotholeTable from "../components/PotholeTable.vue";
import GoogleMap from "../components/GoogleMap.vue";
import axios from "axios";

export default {
  components: {
    PotholeTable,
    GoogleMap,
  },
  data() {
    return {
      potholes: [],
      intervalId: null,
    };
  },
  mounted() {
    this.fetchPotholes();
    this.intervalId = setInterval(this.fetchPotholes, 1000);
  },
  beforeUnmount() {
    if (this.intervalId) {
      clearInterval(this.intervalId);
    }
  },
  methods: {
    async fetchPotholes() {
      try {
        const response = await axios.get(
          `${import.meta.env.VITE_BACKEND_URL}/api/potholes`
        );
        this.potholes = response.data;
      } catch (error) {
        console.error("Error fetching potholes:", error);
      }
    },
  },
};
</script>

<style scoped>
.dashboard {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  padding: 20px;
  height: 100vh;
  background-color: #f5f5f5;
}

.table-container {
  overflow: auto;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.map-container {
  height: 100%;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}
</style>
