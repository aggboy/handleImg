<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <myExample :imgUrl="imgUrl" />
  </div>
</template>

<script>
import myExample from './components/example.vue'

export default {
  name: 'App',
  imgUrl: 'https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=4018557288,1217151095&fm=26&gp=0.jpg',
  components: {
    myExample
  }
}
</script>

<style>
</style>
