<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  created() {
    window.addEventListener("storage", event => {
      const credentials = JSON.parse(
        window.localStorage.getItem("CREDENTIALS_TOKEN")
      );
      console.log(credentials);
      if (event.key === "REQUESTING_SHARED_CREDENTIALS" && credentials) {
        window.localStorage.setItem(
          "CREDENTIALS_SHARING",
          JSON.stringify({ token: "any-token-you-want" })
        );
        window.localStorage.removeItem("CREDENTIALS_SHARING");
      }
      if (event.key === "CREDENTIALS_SHARING" && !credentials) {
        window.sessionStorage.setItem("CREDENTIALS_TOKEN", event.newValue);
      }
    });

    window.localStorage.setItem(
      "REQUESTING_SHARED_CREDENTIALS",
      Date.now().toString()
    );
    window.localStorage.removeItem("REQUESTING_SHARED_CREDENTIALS");
  },
  beforeDestroy() {
    window.removeEventListener("storage", () => {});
  }
};
</script>
<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
