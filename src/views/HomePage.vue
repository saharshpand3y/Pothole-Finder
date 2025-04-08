<template>
  <div class="home" :style="backgroundStyle">
    <div class="blur-overlay"></div>
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
        backgroundImage: "url(https://i.ibb.co/sdT5zfWy/background.jpg)",
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
@import url("https://fonts.googleapis.com/css2?family=Boldonse&family=Host+Grotesk:ital,wght@0,300..800;1,300..800&family=Open+Sans:ital,wght@0,300..800;1,300..800&family=Playwrite+AU+SA:wght@100..400&family=Playwrite+VN:wght@100..400&family=Sacramento&family=Sour+Gummy:ital,wght@0,100..900;1,100..900&family=Winky+Sans:ital,wght@0,300..900;1,300..900&display=swap");

.home {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  position: relative;
}

.blur-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  background-color: rgba(0, 0, 0, 0.2);
}

.content {
  text-align: center;
  position: relative;
  z-index: 1;
}

h1 {
  font-size: 5rem;
  margin-bottom: 4rem;
  min-height: 6rem;
  font-family: "Winky Sans", sans-serif;
  font-optical-sizing: auto;
  font-weight: 400;
  font-style: normal;
  color: white;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.8);
  /* text-transform: uppercase; */
  letter-spacing: 0.1rem;
  text-align: center;
  line-height: 1.2;
  padding: 0;
  /* animation: fadeIn 1s ease-in-out;
  animation-delay: 0.5s;
  animation-fill-mode: forwards;
  animation-iteration-count: infinite;
  animation-direction: alternate; */
  transition: all 0.5s ease;
}

h1:hover {
  animation: none;
  color: #00a08c;
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
  transition: background-color 0.5s ease;
  border-radius: 5px;
  font-family: "Winky Sans", sans-serif;
  font-optical-sizing: auto;
  font-weight: 400;
  font-style: normal;
  text-shadow: 0 0 10px rgba(0, 0, 0, 0.8);
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

button:hover {
  background-color: #00a08c;
  box-shadow: 0px 6px 8px rgba(0, 0, 0, 0.15);
}
</style>
