<template>
  <nav>
    <router-link to="/">지금 뉴스</router-link> |
    <router-link to="/scrap">나만의 뉴스</router-link>
  </nav>
  <router-view/>
</template>

<style lang="scss">
body{
  background-color: #ececec;
  //background-color: rgb(243, 243, 243);
  width: 430px;
  margin: 0 auto;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width:100%;
  min-height: 893px;
  height: auto;
  margin: 30px auto;
  border-radius: 20px;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.08);
  background-color: rgb(243, 243, 243);
  padding:20px;
  box-sizing: border-box;
  position:relative;
}

nav {
  padding: 15px 15px 15px 0;
  font-size: 22px;
  text-align: left;
  a {
    font-weight: bold;
    color: #2c3e50;
    text-decoration: none;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
