<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">获取服务器上的流程图并显示</router-link>|
      <router-link to="./about">只显示xml流程图不可操作</router-link>|
      <router-link to="./empty">新建空的图并且可以以SVG image格式、BPMN diagram格式下载在本地</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
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
