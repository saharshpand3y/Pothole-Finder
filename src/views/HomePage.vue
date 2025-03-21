<template>
  <div class="home" :style="backgroundStyle">
    <div class="content">
      <h1>Find Potholes Near You</h1>
      <button @click="$router.push('/dashboard')">SEARCH NOW</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomePage",
  computed: {
    backgroundStyle() {
      return {
        backgroundImage: "url(src/assets/background.jpeg)",
        backgroundSize: "cover",
        backgroundPosition: "center",
      };
    },
  },
};
</script>

<style scoped>
.home {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
}

.content {
  text-align: center;
}

h1 {
  font-size: 4rem;
  margin-bottom: 2rem;
}

button {
  padding: 1rem 2rem;
  font-size: 1.2rem;
  background-color: #00bfa5;
  color: white;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #00a08c;
}
</style>
