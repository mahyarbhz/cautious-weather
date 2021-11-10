<template>
  <div id="nav">
    <h1>
      <router-link to="/">Cautious Weather</router-link>
    </h1>
  </div>
  <router-view/>
</template>

<style src="./assets/main.scss" lang="scss"></style>

<style lang="scss">
#app {
  font-family: 'Nunito', sans-serAvenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: rgb(26, 30, 243);
      text-decoration: none;
      font-weight: 800;
    }
  }
}
</style>
