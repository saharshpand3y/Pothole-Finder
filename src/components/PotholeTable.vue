<template>
  <table class="pothole-table">
    <thead>
      <tr>
        <th>Pothole ID</th>
        <th>Coordinates</th>
        <th>Severity</th>
        <th>Location</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="pothole in potholes" :key="pothole._id">
        <td>{{ pothole.pothole_id }}</td>
        <td>{{ formatCoordinates(pothole.latitude, pothole.longitude) }}</td>
        <td class="severity" :class="pothole.severity">{{ pothole.severity }}</td>
        <td>{{ pothole.place }}</td>
      </tr>
      <tr v-if="potholes.length === 0">
        <td colspan="4" class="no-data">No potholes found</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "PotholeTable",
  props: {
    potholes: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    formatCoordinates(lat, lng) {
      return `${parseFloat(lat).toFixed(6)}, ${parseFloat(lng).toFixed(6)}`;
    },
  },
};
</script>

<style scoped>
.pothole-table {
  width: 100%;
  border-collapse: collapse;
  background: white;
}

th, td {
  padding: 12px;
  text-align: left;
  border: 1px solid #ddd;
}

th {
  background-color: #333;
  color: white;
}

.severity {
  text-transform: uppercase;
}

.severity.high {
  color: #dc3545;
}

.severity.medium {
  color: #ffc107;
}

.severity.low {
  color: #28a745;
}

.no-data {
  text-align: center;
  color: #666;
  font-style: italic;
}
</style>
