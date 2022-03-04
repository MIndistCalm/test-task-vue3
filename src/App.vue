<template>
  <!-- TODO сделать динамическуюсмену дизайна страницыпо времени -->
  <nav :class="Date().getHours > 16 ? 'light' : 'dark'">
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view />
</template>

<script>
export default {
  name: "app",
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  // background-image: radial-gradient(#e6f2ff, #cee6ff);
  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
nav.light {
  background-image: radial-gradient(#bae412, #9fdb3d);
}
nav.dark {
  background-image: radial-gradient(#224872, #001a35);
}
</style>
