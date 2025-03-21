<template>
  <table class="pothole-table">
    <thead>
      <tr>
        <th>Pothole</th>
        <th>Coordinate</th>
        <th>Severity</th>
        <th>Location</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="pothole in potholes" :key="pothole.id">
        <td>{{ pothole.id }}</td>
        <td>{{ pothole.coordinate }}</td>
        <td>{{ pothole.severity }}</td>
        <td>{{ pothole.location }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "PotholeTable",
  data() {
    return {
      potholes: [
        {
          id: "A",
          coordinate: "123,23",
          severity: "LOW",
          location: "Near Flamingo",
        },
        {
          id: "B",
          coordinate: "123,23",
          severity: "LOW",
          location: "Nandankanan Rd",
        },
        {
          id: "C",
          coordinate: "123,23",
          severity: "MEDIUM",
          location: "Red Town",
        },
      ],
    };
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
</style>
