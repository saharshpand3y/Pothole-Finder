<template>
  <table class="pothole-table">
    <thead>
      <tr>
        <th>Pothole ID</th>
        <th>Coordinates</th>
        <th>Severity</th>
        <th>Location</th>
        <th>Action</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="pothole in potholes" :key="pothole._id">
        <td>{{ pothole.pothole_id }}</td>
        <td>{{ formatCoordinates(pothole.latitude, pothole.longitude) }}</td>
        <td class="severity" :class="pothole.severity">
          {{ pothole.severity }}
        </td>
        <td>{{ pothole.place }}</td>
        <td>
          <button
            class="delete-btn"
            @click="deletePothole(pothole._id)"
            :disabled="isDeleting === pothole._id"
          >
            {{ isDeleting === pothole._id ? "Deleting..." : "Delete" }}
          </button>
        </td>
      </tr>
      <tr v-if="potholes.length === 0">
        <td colspan="5" class="no-data">No potholes found</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from "axios";

export default {
  name: "PotholeTable",
  props: {
    potholes: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      isDeleting: null,
    };
  },
  methods: {
    formatCoordinates(lat, lng) {
      return `${parseFloat(lat).toFixed(6)}, ${parseFloat(lng).toFixed(6)}`;
    },
    async deletePothole(id) {
      try {
        this.isDeleting = id;
        await axios.delete(`${import.meta.env.VITE_BACKEND_URL}/api/potholes/${id}`);
        this.$emit("pothole-deleted");
      } catch (error) {
        console.error("Error deleting pothole:", error);
        alert("Failed to delete pothole");
      } finally {
        this.isDeleting = null;
      }
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

th,
td {
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
.delete-btn {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.delete-btn:hover {
  background-color: #c82333;
}

.delete-btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}
</style>
