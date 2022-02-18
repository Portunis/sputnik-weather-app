<template>
  <div id="nav">
    <router-link to="/">Погода</router-link>
    <router-link to="/photo">Фотографии</router-link>
  </div>
  <router-view />
</template>

<style lang="scss">
*,
html {
  padding: 0;
  margin: 0;
}
body {
  background: linear-gradient(
    90deg,
    rgba(148, 187, 233, 1) 25%,
    rgba(238, 174, 202, 1) 86%
  );
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

  padding: 30px;
}

#nav {
  margin-bottom: 10px;
  a {
    margin: 5px;
    text-decoration: none;
    color: #ffffff;
    font-family: Sarabun, sans-serif;
    font-style: normal;
    font-weight: 500;
    font-size: 25px;
    line-height: 32px;
    text-align: center;

    &.router-link-exact-active {
      color: #000;
    }
  }
}
</style>
