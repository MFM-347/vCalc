<template>
  <div id="app">
    <CalcView />
    <a
      href="https://github.com/MFM-347/vCalc"
      class="github-fork-ribbon fixed"
      data-ribbon="Fork me on GitHub"
      title="Fork me on GitHub"
      >Fork me on GitHub</a
    >
  </div>
</template>

<script setup>
import CalcView from './components/CalcView.vue'
</script>

<style>
@import url('https://unpkg.com/github-fork-ribbon-css@0.2.3/gh-fork-ribbon.css');
body {
  background: linear-gradient(90deg, #7f7fd5 0%, #91eae4 100%);
  color: #212529;
  height: 100vh;
  overflow: hidden;
  padding: 0;
  margin: 0;
  display: grid;
  place-items: center;
}
</style>
<style scoped>
* {
  box-sizing: border-box;
}
.github-fork-ribbon:before {
  background-color: #1c1b22;
}
</style>
