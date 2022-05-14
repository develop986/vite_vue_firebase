<script lang="tsx">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import BootstrapVue from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import Board from './components/Board.vue'

export default {
  name: 'App',
  components: {
    Board,
    BootstrapVue
  }
}
</script>

<template>
  <Board />
</template>