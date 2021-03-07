<template>
  <p>{{version}}</p>
  <router-view/>
</template>

<script>
import { ref } from 'vue'
export default {
  setup () {
    return {
      version: ref(process.env.VUE_APP_VERSION)
    }
  }
}
</script>

<style>
</style>
