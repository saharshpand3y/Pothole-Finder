<template>
  <div class="dashboard">
    <div class="table-container">
      <PotholeTable :potholes="potholes" />
    </div>
    <div class="map-container">
      <GoogleMap :potholes="potholes" />
    </div>
  </div>
</template>

<script>
import PotholeTable from "../components/PotholeTable.vue";
import GoogleMap from "../components/GoogleMap.vue";

export default {
  name: "DashboardPage",
  components: {
    PotholeTable,
    GoogleMap,
  },
  data() {
    return {
      potholes: [
        {
          id: "A",
          coordinate: "20.360236,85.8269268", // Nandankanan Road
          severity: "LOW",
          location: "Near Flamingo",
        },
        {
          id: "B",
          coordinate: "20.358125,85.833215", // Different location on Nandankanan Rd
          severity: "HIGH",
          location: "Nandankanan Rd",
        },
        {
          id: "C",
          coordinate: "20.355896,85.830469", // Red Town area
          severity: "MEDIUM",
          location: "Red Town",
        },
      ],
    };
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
