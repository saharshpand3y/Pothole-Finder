<template>
  <div class="home" :style="backgroundStyle">
    <div class="content">
      <h1><span ref="typedText"></span></h1>
      <button @click="$router.push('/dashboard')">SEARCH NOW</button>
    </div>
  </div>
</template>

<script>
import Typed from "typed.js";

export default {
  name: "HomePage",
  data() {
    return {
      typed: null,
    };
  },
  computed: {
    backgroundStyle() {
      return {
        backgroundImage: "url(src/assets/background.jpeg)",
        backgroundSize: "cover",
        backgroundPosition: "center",
      };
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.typed = new Typed(this.$refs.typedText, {
        strings: [
          "Find Potholes Near You",
          "अपने आस-पास गड्ढे खोजें",
          "ଆପଣଙ୍କ ନିକଟରେ ଥିବା ଗାତ ଖୋଜନ୍ତୁ",
        ],
        typeSpeed: 50,
        backSpeed: 30,
        backDelay: 2000,
        loop: true,
        showCursor: true,
        cursorChar: "|",
      });
    });
  },
  beforeUnmount() {
    if (this.typed) {
      this.typed.destroy();
    }
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Boldonse&family=Host+Grotesk:ital,wght@0,300..800;1,300..800&family=Open+Sans:ital,wght@0,300..800;1,300..800&family=Playwrite+AU+SA:wght@100..400&family=Playwrite+VN:wght@100..400&family=Sacramento&family=Sour+Gummy:ital,wght@0,100..900;1,100..900&display=swap");
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
  font-size: 5rem;
  margin-bottom: 4rem;
  min-height: 6rem; /* Added to prevent layout shift */
  /* font-family: "Boldonse", system-ui;
  font-weight: 400;
  font-style: normal; */
}

.typed-cursor {
  font-size: 4rem;
  color: white;
}

button {
  padding: 1rem 2rem;
  font-size: 1.2rem;
  background-color: #00bfa5;
  color: white;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
  border-radius: 5px;
}

button:hover {
  background-color: #00a08c;
}
</style>
