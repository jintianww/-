<template>
  <router-view v-slot="{Component}" class="page">
    
      <transition name="fa">
        <component :is="Component"></component>
      </transition>

  </router-view>
</template>

<style lang="scss">
.fa-enter-active,
.fa-leave-active {
  transition: all 0.5s linear;
  transform-origin: center left;
}
.fa-enter-from {
  left: 100% !important;
}
.fa-leave-to {
  left: 100%;
  transform: rotate(60deg);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: relative;
}
.page {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
}
body,
html,
#app {
  width: 100%;
  height: 100%;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
